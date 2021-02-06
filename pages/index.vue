<template>
  <div class="bg-gray-800 text-gray-300 w-full min-h-screen ">
    <div class="max-w-screen-lg w-4/5 xl:w-full mx-auto antialiased flex flex-col items-center">
      <h1 class="font-extrabold text-6xl pt-20">Focus Visible Sample</h1>

      <h2 class="text-bold text-xl mt-12 max-w-lg md:text-center">
        This submit button just get a focus ring  when  focused by keyboard
        interations (TAB) . 
      </h2>
      <form class="mt-10 w-full max-w-sm "  @submit.prevent="">
        <fieldset >
          <label for="text" class="block ml-4">Pet Name</label>
          <input
            type="text"
            placeholder="type your pet name here"
            class="block mt-2 px-4 py-2 w-full rounded-lg bg-gray-700 shadow-md focus:outline-none focus:ring focus-visible:ring-2 focus-visible:ring-indigo-600 "
          />
        </fieldset>
        
        <div class="mt-6 w-full text-center">
          <button class="py-2  w-full  px-4 rounded-lg bg-indigo-700 hover:bg-indigo-800 shadow-lg focus:outline-none  focus-visible:ring-2 focus-visible:ring-indigo-400">
            Get your Free Food
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {};
</script>

