<!--
 Copyright 2023 Google LLC

 Licensed under the Apache License, Version 2.0 (the "License");
 you may not use this file except in compliance with the License.
 You may obtain a copy of the License at

      https://www.apache.org/licenses/LICENSE-2.0

 Unless required by applicable law or agreed to in writing, software
 distributed under the License is distributed on an "AS IS" BASIS,
 WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 See the License for the specific language governing permissions and
 limitations under the License.
 -->

 <script lang="ts">
  import Dropdown from './Dropdown.svelte';

  export let month: number;
  export let day: number;
  export let numCols = 7;
  export let onChange: (month: number, day: number) => void = () => {};

  let opened = false;
  let dataLayersClicked = false; // Track if "Data Layers endpoint" is clicked

  const monthDays: Record<string, number> = {
    January: 31,
    February: 28,
    March: 31,
    April: 30,
    May: 31,
    June: 30,
    July: 31,
    August: 31,
    September: 30,
    October: 31,
    November: 30,
    December: 31,
  };

  const months = Object.keys(monthDays);

  function dayFrom(row: number, col: number) {
    return row * numCols + col + 1;
  }
</script>

<div class="relative">
  <md-text-button class="w-full" trailing-icon role={undefined} on:click={() => (opened = !opened)}>
    <div class="flex items-center">
      <md-icon>event</md-icon>
      <span>&nbsp; {months[month]} {day}</span>
    </div>
  </md-text-button>

  {#if opened}
    <div class="px-4 pb-4">
      {#if !dataLayersClicked}
        <Dropdown
          value="annual Sunshine"
          options={{ "annual Sunshine": "annual Sunshine" }}
          onChange={async (value) => {
            // No need to update month as we are only showing one option
            onChange(month, day);
          }}
        />
      {:else}
        <div class="disabled-dropdown">
          <span>annual Sunshine</span>
        </div>
      {/if}
    </div>
  
    <table>
      {#each [...Array(Math.ceil(monthDays[months[month]] / numCols)).keys()] as row}
        <tr>
          {#each [...Array(numCols).keys()] as col}
            <td>
              {#if day == dayFrom(row, col)}
                <button
                  class="primary on-primary-text relative w-8 h-8 rounded-full"
                  disabled
                >
                  <md-ripple />
                  {dayFrom(row, col)}
                </button>
              {:else if dayFrom(row, col) <= monthDays[months[month]]}
                <button
                  class="relative w-8 h-8 rounded-full"
                  disabled
                >
                  <md-ripple />
                  {dayFrom(row, col)}
                </button>
              {/if}
            </td>
          {/each}
        </tr>
      {/each}
    </table>
  {/if}
</div>

<!-- Button to toggle Data Layers endpoint -->
<button on:click={() => (dataLayersClicked = !dataLayersClicked)}>
  Data Layers endpoint
</button>

{#if dataLayersClicked}
  <div>
    <h3>Annual Sunshine</h3>
    <!-- Render solar panels and roof only -->
    <p>Solar panels and roof only are shown here.</p>
  </div>
{/if}