<script setup lang="ts">
const user = useSupabaseUser();
const auth = ref(true);

const logout = async () => {
  await useSupabaseClient().auth.signOut();
  await navigateTo("/login");
};
</script>

<template>
  <nav
    class="fixed z-20 top-0 start-0 flex justify-between px-40 items-center w-full p-4"
  >
    <div></div>
    <div class="flex justify-between items-center gap-1">
      <ColorModeToggle />
      <Button
        v-if="
          !user &&
          $router.currentRoute.value.name != 'login' &&
          $router.currentRoute.value.name != 'signup'
        "
        variant="secondary"
        as-child
      >
        <NuxtLink to="/login"> Login </NuxtLink>
      </Button>

      <DropdownMenu v-if="user">
        <DropdownMenuTrigger>
          <Avatar size="sm">
            <AvatarImage
              src="https://imagess.pexels.com/photos/326055/pexels-photo-326055.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1"
              alt="@radix-vue"
            />
            <AvatarFallback>
              <Icon name="ooui:user-avatar-outline" size="15" />
            </AvatarFallback>
          </Avatar>
        </DropdownMenuTrigger>
        <DropdownMenuContent>
          <DropdownMenuLabel>My Account</DropdownMenuLabel>
          <DropdownMenuSeparator />
          <DropdownMenuItem>Profile</DropdownMenuItem>
          <DropdownMenuItem>Billing</DropdownMenuItem>
          <DropdownMenuItem>Team</DropdownMenuItem>
          <DropdownMenuSeparator />
          <DropdownMenuItem class="cursor-pointer" @click="logout">
            <span>Log out</span>
            <DropdownMenuShortcut>⇧⌘Q</DropdownMenuShortcut>
          </DropdownMenuItem>
        </DropdownMenuContent>
      </DropdownMenu>
    </div>
  </nav>
</template>
