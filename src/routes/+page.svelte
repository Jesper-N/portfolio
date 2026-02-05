<script lang="ts">
    import * as Item from "$lib/components/ui/item/index.js";
    import { Button } from "$lib/components/ui/button/index.js";
</script>

<main class="min-h-screen bg-background text-foreground">
    <Item.Root variant="outline" class="flex w-md">
        <Item.Content>
            <Item.Title>Basic Item</Item.Title>
            <Item.Description
                >A simple item with title and description.</Item.Description
            >
        </Item.Content>
        <Item.Actions>
            <Button variant="outline" size="sm">Action</Button>
        </Item.Actions>
    </Item.Root>
</main>
