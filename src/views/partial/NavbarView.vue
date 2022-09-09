<template>
  <nav class="relative flex flex-wrap items-center justify-between px-2 py-3 navbar-expand-lg bg-indigo-600 mb-3">
    <div class="container px-4 mx-auto flex flex-wrap items-center justify-between">
      <div class="w-full relative flex justify-between lg:w-auto  px-4 lg:static lg:block lg:justify-start">
        <a
          class="text-sm font-bold leading-relaxed inline-block mr-4 py-2 whitespace-no-wrap uppercase text-gray-100"
          href="#"
        >
          Whatsapp Service Manager
        </a>
        <button
          class="cursor-pointer text-xl leading-none px-3 py-1 border border-solid border-transparent rounded bg-transparent block lg:hidden outline-none focus:outline-none"
          type="button"
        >
          <span class="block relative w-6 h-px rounded-sm bg-gray-100" />
          <span class="block relative w-6 h-px rounded-sm bg-gray-100 mt-1" />
          <span class="block relative w-6 h-px rounded-sm bg-gray-100 mt-1" />
        </button>
      </div>
      <div class="lg:flex flex-grow items-center">
        <ul class="flex flex-col lg:flex-row list-none ml-auto">
          <li class="nav-item">
            <router-link
              to="/"
              class="px-3 py-2 flex items-center text-xs uppercase font-bold leading-snug text-gray-100 hover:opacity-75"
            >
              Dashboard
            </router-link>
          </li>
          <li class="nav-item">
            <router-link
              to="/devices"
              class="px-3 py-2 flex items-center text-xs uppercase font-bold leading-snug text-gray-100 hover:opacity-75"
            >
              Manage Device
            </router-link>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>