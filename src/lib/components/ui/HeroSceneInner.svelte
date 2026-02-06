<script lang="ts">
    import { T, useTask } from "@threlte/core";
    import { Float } from "@threlte/extras";
    import { spring } from "svelte/motion";
    import {
        TorusKnotGeometry,
        ShaderMaterial,
        AdditiveBlending,
        Color,
        type Mesh,
        type Group
    } from "three";

    let { mouseX = 0, mouseY = 0 }: { mouseX?: number; mouseY?: number } = $props();

    let meshRef: Mesh | undefined = $state(undefined);
    let groupRef: Group | undefined = $state(undefined);

    const geometry = new TorusKnotGeometry(1.2, 0.35, 200, 32);

    const vertexShader = `
        varying vec3 vNormal;
        varying vec3 vPosition;
        varying vec3 vWorldPosition;

        void main() {
            vNormal = normalize(normalMatrix * normal);
            vPosition = position;
            vec4 worldPos = modelMatrix * vec4(position, 1.0);
            vWorldPosition = worldPos.xyz;
            gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
        }
    `;

    const fragmentShader = `
        uniform float uTime;
        uniform vec3 uColorA;
        uniform vec3 uColorB;
        uniform vec3 uColorC;

        varying vec3 vNormal;
        varying vec3 vPosition;
        varying vec3 vWorldPosition;

        void main() {
            float fresnel = pow(1.0 - abs(dot(vNormal, vec3(0.0, 0.0, 1.0))), 2.5);

            float sweep = sin(vPosition.x * 2.0 + vPosition.y * 1.5 + uTime * 0.4) * 0.5 + 0.5;
            float pulse = sin(uTime * 0.3) * 0.5 + 0.5;

            vec3 color = mix(uColorA, uColorB, sweep);
            color = mix(color, uColorC, fresnel * 0.6);

            float alpha = fresnel * 0.7 + 0.08;
            alpha += sweep * 0.05;
            alpha *= 0.9 + pulse * 0.1;

            gl_FragColor = vec4(color, alpha);
        }
    `;

    const shaderMaterial = new ShaderMaterial({
        vertexShader,
        fragmentShader,
        uniforms: {
            uTime: { value: 0 },
            uColorA: { value: new Color("#a0c4ff") },
            uColorB: { value: new Color("#e0e0e0") },
            uColorC: { value: new Color("#caf0f8") },
        },
        wireframe: true,
        transparent: true,
        blending: AdditiveBlending,
        depthWrite: false,
    });

    const tiltX = spring(0, { stiffness: 0.04, damping: 0.7 });
    const tiltY = spring(0, { stiffness: 0.04, damping: 0.7 });

    $effect(() => {
        tiltX.set(mouseY * 0.3);
        tiltY.set(mouseX * 0.4);
    });

    let autoRotation = 0;

    useTask((delta) => {
        shaderMaterial.uniforms.uTime.value += delta;
        autoRotation += delta * 0.15;

        if (groupRef) {
            groupRef.rotation.x = $tiltX;
            groupRef.rotation.y = $tiltY + autoRotation;
        }
    });
</script>

<T.PerspectiveCamera makeDefault position={[0, 0, 4.5]} fov={45} />

<T.AmbientLight intensity={0.15} />

<T.Group bind:ref={groupRef}>
    <Float
        floatIntensity={1.5}
        speed={2}
        rotationIntensity={0}
        rotationSpeed={0}
    >
        <T.Mesh
            bind:ref={meshRef}
            {geometry}
            material={shaderMaterial}
            scale={0.85}
        />
    </Float>
</T.Group>
