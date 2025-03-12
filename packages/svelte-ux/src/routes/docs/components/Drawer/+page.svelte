<script lang="ts">
  import { createEventDispatcher } from 'svelte';
  import { mdiMagnify } from '@mdi/js';

  import {
    Button,
    Drawer,
    MultiSelect
  } from 'svelte-ux';

  import Preview from '$lib/components/Preview.svelte';
  import Blockquote from '$docs/Blockquote.svelte';

  import { writable } from 'svelte/store';

  // Generate 100 sample options
  const options = Array.from({ length: 100 }, (_, i) => ({
    label: `Option ${i + 1}`,
    value: i
  }));

  const dispatch = createEventDispatcher<{
    change: { value: any[] };
  }>();

  type FormatSelectedOptions = {
    options: Array<{ label: string }>;
  };

  const formatSelected = ({ options }: FormatSelectedOptions) =>
    options.map((o) => o.label).join(', ') || '';

  let value = writable<unknown[]>([]);

	let bugOpen = false;
	let fixOpen = false;
</script>

<h1>Examples</h1>

<h2>Bug</h2>

<Preview>
  <Button on:click={() => (bugOpen = true)}>Open ResponsiveMenu</Button>
  <Drawer
    open={bugOpen}
    placement="bottom"
    class="ResponsiveMenu p-4 max-h-[50vh]"
    on:close={() => (bugOpen = false)}
  >
    <div class="container">
      <MultiSelect
        id="test-id"
        {options}
        value={$value ?? []}
        placeholder="Select an option"
        onApply={async (data) => {
          $value = data.selection.selected;
          dispatch('change', { value: data.value });
        }}
        {formatSelected}
        inlineSearch
        infiniteScroll
        perPage={50}
        max={4}
        classes={{
          search: 'p-2',
          options: 'px-2',
          actions: 'p-2',
        }}
        maintainOrder
      />
    </div>
  </Drawer>
</Preview>

<h2>Fix</h2>

<Preview>
  <Button on:click={() => (fixOpen = true)}>Open ResponsiveMenu</Button>
  <Drawer
    open={fixOpen}
    placement="bottom"
    class="ResponsiveMenu max-h-[50vh] flex flex-col"
    on:close={() => (fixOpen = false)}
  >
    <MultiSelect
      id="test-id-2"
      {options}
      value={$value ?? []}
      placeholder="Select an option"
      onApply={async (data) => {
        $value = data.selection.selected;
        dispatch('change', { value: data.value });
      }}
      {formatSelected}
      inlineSearch
      infiniteScroll
      perPage={50}
      max={4}
      classes={{
        root: 'flex flex-col min-h-0',
        search: 'p-2 shrink-0',
        options: 'px-2 flex-1 overflow-auto min-h-0',
        actions: 'p-2 border-t shrink-0',
      }}
      maintainOrder
    />
  </Drawer>
</Preview>
