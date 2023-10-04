<script lang="ts">
  import { Label, Select } from "flowbite-svelte";

  let gradientColors = ["#FF5733", "#6A82FB"];
  let gradientType = "linear-gradient";
  let gradientCode = "";
  let showCopyNotification = false;

  function generateGradient() {
    gradientCode = `${gradientType}(${gradientColors.join(", ")})`;
  }

  function copyCSS() {
    const cssText = `${gradientType}(${gradientColors.join(", ")})`;
    const el = document.createElement("textarea");
    el.value = cssText;
    document.body.appendChild(el);
    el.select();
    document.execCommand("copy");
    document.body.removeChild(el);
    showCopyNotification = true;

    // Hide the notification after a few seconds (e.g., 3 seconds)
    setTimeout(() => {
      showCopyNotification = false;
    }, 3000);
  }

  function updateGradient() {
    generateGradient();
  }

  function generateRandomGradient() {
    gradientColors = [getRandomColor(), getRandomColor()];
    generateGradient();
  }

  function getRandomColor() {
    const letters = "0123456789ABCDEF";
    let color = "#";
    for (let i = 0; i < 6; i++) {
      color += letters[Math.floor(Math.random() * 16)];
    }
    return color;
  }
</script>

<main class="min-h-screen flex items-center justify-center">
  <div class="w-full max-w-4xl p-8 bg-white rounded-lg shadow-md dark:bg-gray-800 flex">
    <!-- Settings (Left Half) -->
    <div class="w-1/2 p-4">
      <h1 class="text-2xl font-semibold text-gray-800 dark:text-white">CSS Gradient Generator</h1>

      <!-- Gradient Type Selector -->
      <div class="mt-4">
        <Label for="gradientType" class="text-gray-700 dark:text-gray-400">Gradient Type:</Label>
        <Select
          id="gradientType"
          bind:value={gradientType}
          class="mt-1"
        >
          <option value="linear-gradient">Linear Gradient</option>
          <option value="radial-gradient">Radial Gradient</option>
        </Select>
      </div>

      <!-- Color Pickers -->
      <div class="mt-4">
        <Label class="text-gray-700 dark:text-gray-400">Gradient Colors:</Label>
        <div class="flex space-x-2">
          {#each gradientColors as color, index}
            <input
              type="color"
              bind:value={gradientColors[index]}
              class="mt-1"
              on:input={updateGradient}
            />
          {/each}
        </div>
      </div>

      <!-- Button Container -->
      <div class="mt-4 flex items-center">
        <!-- Generate and Random Buttons -->
        
          <button
            type="button"
            class="text-white bg-blue-700 hover:bg-blue-800 focus:outline-none focus:ring-4 focus:ring-blue-300 font-medium rounded-full text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
            on:click={generateGradient}
          >
            Generate Gradient
          </button>
          <button
            type="button"
            class="text-white bg-blue-700 hover:bg-blue-800 focus:outline-none focus:ring-4 focus:ring-blue-300 font-medium rounded-full text-sm px-5 py-2.5 text-center ml-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
            on:click={generateRandomGradient}
          >
            Random
          </button>
        </div>

        <!-- Copy CSS Button -->
        <button
          type="button"
          class="text-white bg-green-500 hover:bg-green-600 focus:outline-none focus:ring-4 focus:ring-green-300 font-medium rounded-full text-sm px-5 py-2.5 text-center dark:bg-green-600 dark:hover:bg-green-700 dark:focus:ring-green-800 mt-2"
          on:click={copyCSS}
        >
          Copy CSS
        </button>

        <!-- Notification (Initially hidden) -->
        {#if showCopyNotification}
          <div class="text-sm text-green-600 mt-2">CSS Copied</div>
        {/if}
      
    </div>

    <!-- Gradient Display (Right Half) -->
    <div class="w-1/2 p-4">
      <div
        class="w-full h-64"
        style="background-image: {gradientCode};"
      ></div>
    </div>
  </div>
</main>
