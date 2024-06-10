<script setup lang="ts">
const supabase = useSupabaseClient();
const email = ref("");
const password = ref("");
const loading = ref(false);
const err = ref();

const signin = async () => {
  err.value = null;
  loading.value = true;
  const { error, data } = await supabase.auth.signInWithPassword({
    email: email.value,
    password: password.value,
  });

  if (error) {
    loading.value = false;
    err.value = error.message;
  }
  if (useSupabaseUser().value) {
    loading.value = false;
    await navigateTo("/");
  }
};
</script>

<template>
  <main class="w-screen h-screen flex flex-col items-center justify-center">
    <Card class="">
      <CardHeader class="space-y-1">
        <CardTitle class="text-2xl">Sign in to Whatever</CardTitle>
        <CardDescription>
          Enter your email below to sign in to your account
        </CardDescription>
      </CardHeader>
      <CardContent class="grid gap-4">
        <CardDescription v-if="err" class="text-red-500">
          {{ err }}
        </CardDescription>
        <div class="grid gap-2">
          <Label for="email">Email</Label>
          <Input
            id="email"
            type="email"
            placeholder="m@example.com"
            v-model="email"
          />
        </div>
        <div class="grid gap-2">
          <Label for="password">Password</Label>
          <Input id="password" type="password" v-model="password" />
        </div>
        <Button class="w-full" @click="signin" v-if="!loading">
          Sign in
        </Button>
        <Button class="w-full" disabled v-if="loading">
          <Icon
            name="fluent:arrow-sync-20-filled"
            class="animate-spin"
            size="15"
          />"
        </Button>
        <div class="relative">
          <div class="absolute inset-0 flex items-center">
            <span class="w-full border-t" />
          </div>
          <div class="relative flex justify-center text-xs uppercase">
            <span class="bg-background px-2 text-muted-foreground">
              Or continue with
            </span>
          </div>
        </div>
        <div class="grid grid-cols-2 gap-6">
          <Button variant="outline">
            <Icon name="uil:github" class="mr-2 h-5 w-5" />
            Github
          </Button>
          <Button variant="outline">
            <svg role="img" viewBox="0 0 24 24" class="mr-2 h-4 w-4">
              <path
                fill="currentColor"
                d="M12.48 10.92v3.28h7.84c-.24 1.84-.853 3.187-1.787 4.133-1.147 1.147-2.933 2.4-6.053 2.4-4.827 0-8.6-3.893-8.6-8.72s3.773-8.72 8.6-8.72c2.6 0 4.507 1.027 5.907 2.347l2.307-2.307C18.747 1.44 16.133 0 12.48 0 5.867 0 .307 5.387.307 12s5.56 12 12.173 12c3.573 0 6.267-1.173 8.373-3.36 2.16-2.16 2.84-5.213 2.84-7.667 0-.76-.053-1.467-.173-2.053H12.48z"
              />
            </svg>
            Google
          </Button>
        </div>
      </CardContent>
      <CardFooter>
        <CardDescription class="justify-center">
          Don't have an account?
          <a href="/signup" class="text-green-700">Sign up</a>
        </CardDescription>
      </CardFooter>
    </Card>
  </main>
</template>
