<script lang="ts">
  import { createEventDispatcher } from 'svelte';
  import type {
    TableColumnStore,
    TableRecordStore,
    TableOptionsStore,
  } from '@mathesar/stores/tableData';
  import {
    faFilter,
    faSort,
    faListAlt,
    faTrashAlt,
  } from '@fortawesome/free-solid-svg-icons';
  import { States } from '@mathesar/utils/api';
  import { Button, Icon } from '@mathesar-components';

  const dispatch = createEventDispatcher();

  export let columns: TableColumnStore;
  export let records: TableRecordStore;
  export let options: TableOptionsStore;

  export let selectedEntries: string[];

  function openDisplayOptions() {
    dispatch('openDisplayOptions');
  }
</script>

<div class="actions-pane">
  <Button appearance="plain" on:click={openDisplayOptions}>
    <Icon data={faFilter} size="0.8em"/>
    <span>
      Filters
      {#if $options.filter?.filters?.length > 0}
        ({$options.filter?.filters?.length})
      {/if}
    </span>
  </Button>

  <Button appearance="plain" on:click={openDisplayOptions}>
    <Icon data={faSort}/>
    <span>
      Sort
      {#if $options.sort?.size > 0}
        ({$options.sort?.size})
      {/if}
    </span>
  </Button>

  <Button appearance="plain" on:click={openDisplayOptions}>
    <Icon data={faListAlt}/>
    <span>
      Group
      {#if $options.group?.size > 0}
        ({$options.group?.size})
      {/if}
    </span>
  </Button>

  {#if selectedEntries.length > 0}
    <Button appearance="plain" on:click={() => dispatch('deleteRecords')}>
      <Icon data={faTrashAlt}/>
      <span>
        Delete {selectedEntries.length} records
      </span>
    </Button>
  {/if}

  <div class="loading-info">
    {#if $columns.state === States.Loading}
      | Loading table

    {:else if $columns.state === States.Error}
      | Error in loading table: {$columns.error}
    {/if}

    {#if $records.state === States.Loading}
      | Loading records

    {:else if $records.state === States.Error}
      | Error in loading records: {$records.error}
    {/if}
  </div>
</div>
