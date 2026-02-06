<script lang="ts">
    import { onMount } from "svelte";
    import {
        AdditiveBlending,
        AmbientLight,
        Color,
        Group,
        Mesh,
        PerspectiveCamera,
        Scene,
        ShaderMaterial,
        TorusKnotGeometry,
        WebGLRenderer,
    } from "three";

    let container: HTMLDivElement | null = null;
    let mouseX = 0;
    let mouseY = 0;

    function handleMouseMove(e: MouseEvent) {
        const rect = (e.currentTarget as HTMLDivElement).getBoundingClientRect();
        mouseX = ((e.clientX - rect.left) / rect.width) * 2 - 1;
        mouseY = -((e.clientY - rect.top) / rect.height) * 2 + 1;
    }

    function handleMouseLeave() {
        mouseX = 0;
        mouseY = 0;
    }

    onMount(() => {
        if (!container) return;

        const scene = new Scene();
        const camera = new PerspectiveCamera(45, 1, 0.1, 100);
        camera.position.set(0, 0, 5.8);

        const renderer = new WebGLRenderer({
            alpha: true,
            antialias: true,
            powerPreference: "high-performance",
        });
        renderer.setClearColor(0x000000, 0);
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 1.5));
        renderer.domElement.style.width = "100%";
        renderer.domElement.style.height = "100%";
        renderer.domElement.style.display = "block";

        const geometry = new TorusKnotGeometry(1.2, 0.35, 140, 24);
        geometry.center();
        const shaderMaterial = new ShaderMaterial({
            vertexShader: `
                varying vec3 vNormal;
                varying vec3 vPosition;

                void main() {
                    vNormal = normalize(normalMatrix * normal);
                    vPosition = position;
                    gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
                }
            `,
            fragmentShader: `
                uniform float uTime;
                uniform vec3 uColorA;
                uniform vec3 uColorB;
                uniform vec3 uColorC;

                varying vec3 vNormal;
                varying vec3 vPosition;

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
            `,
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

        const mesh = new Mesh(geometry, shaderMaterial);
        mesh.scale.setScalar(0.77);
        mesh.position.set(0, 0, 0);

        const group = new Group();
        group.add(mesh);
        scene.add(group);
        scene.add(new AmbientLight(0xffffff, 0.15));

        const resize = () => {
            if (!container) return;
            const width = Math.max(1, container.clientWidth);
            const height = Math.max(1, container.clientHeight);
            camera.aspect = width / height;
            camera.updateProjectionMatrix();
            renderer.setSize(width, height, false);
        };

        const resizeObserver = new ResizeObserver(resize);
        resizeObserver.observe(container);
        resize();

        renderer.render(scene, camera);
        container.appendChild(renderer.domElement);

        let rafId = 0;
        let lastFrame = performance.now();
        let autoRotation = 0;
        let floatTime = 0;

        const animate = (now: number) => {
            const delta = Math.min((now - lastFrame) / 1000, 0.033);
            lastFrame = now;
            autoRotation += delta * 0.15;
            floatTime += delta * 2;

            shaderMaterial.uniforms.uTime.value += delta;

            const targetX = mouseY * 0.3;
            const targetY = mouseX * 0.4 + autoRotation;
            group.rotation.x += (targetX - group.rotation.x) * 0.08;
            group.rotation.y += (targetY - group.rotation.y) * 0.08;
            mesh.position.y = Math.sin(floatTime) * 0.08;

            renderer.render(scene, camera);
            rafId = requestAnimationFrame(animate);
        };

        rafId = requestAnimationFrame(animate);

        return () => {
            cancelAnimationFrame(rafId);
            resizeObserver.disconnect();
            geometry.dispose();
            shaderMaterial.dispose();
            renderer.dispose();
            if (container?.contains(renderer.domElement)) {
                container.removeChild(renderer.domElement);
            }
        };
    });
</script>

<div
    bind:this={container}
    class="absolute inset-0 z-0"
    role="presentation"
    onmousemove={handleMouseMove}
    onmouseleave={handleMouseLeave}
></div>
