<script lang="ts">
  import type { CommitMetadata } from "@app/commit";
  import { formatCommit } from "@app/utils";
  import { createEventDispatcher } from "svelte";

  import Icon from "@app/Icon.svelte";
  import CommitAuthorship from "./CommitAuthorship.svelte";

  export let commit: CommitMetadata;

  const dispatch = createEventDispatcher();

  function browseCommit(commit: string) {
    dispatch("browseCommit", commit);
  }
</script>

<style>
  .hash {
    font-family: var(--font-family-monospace);
    font-size: 0.75rem;
    padding: 0 1.5rem;
  }
  .commit-teaser {
    background-color: var(--color-foreground-background);
    padding: 0.5rem 0rem;
  }
  .commit-teaser:hover {
    background-color: var(--color-foreground-background-lighter);
  }
  .commit-teaser:first-child {
    border-top-left-radius: 0.25rem;
    border-top-right-radius: 0.25rem;
  }
  .commit-teaser:last-child {
    border-bottom-left-radius: 0.25rem;
    border-bottom-right-radius: 0.25rem;
  }
  .commit-teaser {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .column-left {
    padding-left: 1rem;
    flex: min-content;
  }
  .commit-teaser .column-right {
    display: flex;
    align-items: center;
    padding-right: 1.5rem;
  }
  .summary {
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
    padding-right: 1rem;
  }
  .badge {
    margin: 0;
  }
  .browse {
    display: flex;
    z-index: 10;
    width: 100%;
    height: 100%;
  }

  @media (max-width: 720px) {
    .hash {
      padding-right: 0;
    }
    .column-left {
      overflow: hidden;
    }
    .badge, .browse {
      display: none !important;
    }
    .summary {
      overflow: hidden;
      white-space: nowrap;
      text-overflow: ellipsis;
      padding-right: 1rem;
    }
  }
</style>

<div class="commit-teaser">
  <div class="column-left">
    <div class="header">
      <div class="summary">
        {commit.header.summary}
      </div>
    </div>
    <CommitAuthorship {commit} />
  </div>
  <div class="column-right">
    {#if commit.context.committer}
      <span class="badge tertiary">Verified</span>
    {/if}
    <span class="secondary hash">{formatCommit(commit.header.sha1)}</span>
    <div class="browse" on:click|stopPropagation={() => browseCommit(commit.header.sha1)}>
      <Icon name="browse" width={17} inline fill />
    </div>
  </div>
</div>
