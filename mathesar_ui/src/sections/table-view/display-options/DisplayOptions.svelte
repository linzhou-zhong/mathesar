<script lang="ts">
  import { createEventDispatcher } from 'svelte';
  import { fade } from 'svelte/transition';
  import {
    faTimes,
  } from '@fortawesome/free-solid-svg-icons';
  import { Icon, Button } from '@mathesar-components';
  import type {
    SortOption,
    GroupOption,
    FilterOption,
    TableColumnData,
  } from '@mathesar/stores/tableData';
  import type { SelectOption } from '@mathesar-components/types';
  import FilterSection from './FilterSection.svelte';
  import SortSection from './SortSection.svelte';
  import GroupSection from './GroupSection.svelte';

  const dispatch = createEventDispatcher();

  export let columns: TableColumnData;
  export let sort: SortOption;
  export let group: GroupOption;
  export let filter: FilterOption;

  function getColumnOptions(
    _columns: TableColumnData,
  ): SelectOption[] {
    return _columns?.data?.map((column) => ({
      id: column.name,
      label: column.name,
    })) || [];
  }

  $: columnOptions = getColumnOptions(columns);
</script>

<div class="display-opts" transition:fade|local={{ duration: 250 }}>
  <div class="header">
    <span>Table Display Properties</span>
    <Button class="padding-zero" appearance="ghost" size="medium"
          on:click={() => dispatch('close')}>
      <Icon data={faTimes}/>
    </Button>
  </div>

  <FilterSection options={columnOptions} bind:filter on:reload/>
  <SortSection options={columnOptions} bind:sort on:reload/>
  <GroupSection options={columnOptions} bind:group on:reload/>
</div>
