<script lang="ts">
  import { faSpinner } from '@fortawesome/free-solid-svg-icons';
  import {
    FileUpload,
    Button,
    Icon,
  } from '@mathesar-components';
  import type { FileImport } from '@mathesar/stores/fileImports';
  import { States } from '@mathesar/utils/api';

  import {
    uploadNewFile,
    getFileUploadInfo,
    loadPreview,
  } from '../importUtils';

  export let fileImportStore: FileImport;
</script>

<div>Add Table (Step 1 of 2)</div>
<h2>Import your data</h2>
<div class="help-content">
  Create a table by importing data. Very large data sets can sometimes take some minutes to process.
  Please do not close this tab, you may still open and view other tables in the meanwhile.
</div>

<FileUpload bind:fileUploads={$fileImportStore.uploads}
            fileProgress={getFileUploadInfo($fileImportStore)}
            on:add={(e) => uploadNewFile(fileImportStore, e.detail)}/>

<div class="help-content">
  You can import tabular (CSV, TSV) data.
</div>

<div class="actions">
  <Button appearance="primary"
          disabled={
            $fileImportStore.uploadStatus !== States.Done
            || $fileImportStore.previewTableCreationStatus === States.Loading
          }
          on:click={() => loadPreview(fileImportStore)}>
      Next

    {#if $fileImportStore.previewTableCreationStatus === States.Loading}
      <Icon data={faSpinner} spin={true}/>
    {/if}
  </Button>
</div>
