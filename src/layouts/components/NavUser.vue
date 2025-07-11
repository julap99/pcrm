<script setup lang="ts">
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuGroup,
  DropdownMenuItem,
  DropdownMenuLabel,
  DropdownMenuSeparator,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu";
import {
  SidebarMenu,
  SidebarMenuButton,
  SidebarMenuItem,
  useSidebar,
} from "@/components/ui/sidebar";
import { ChevronsUpDown, LogOut, User, CreditCard } from "lucide-vue-next";

import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
} from "@/components/ui/dialog";
import { Button } from "@/components/ui/button";
import { useAuthStore } from "@/stores/auth";
import { useRouter } from "vue-router";
import { ref } from "vue";

const authStore = useAuthStore();
const router = useRouter();
const { isMobile } = useSidebar();
const isLogoutDialogOpen = ref(false);
const isDropdownOpen = ref(false);

function showLogoutConfirmation() {
  isLogoutDialogOpen.value = true;
  // Close the dropdown when opening the dialog
  isDropdownOpen.value = false;
}

function handleLogout() {
  authStore.logout();
  isLogoutDialogOpen.value = false;
}

function closeLogoutDialog() {
  isLogoutDialogOpen.value = false;
}

function navigateToSupport() {
  router.push("/support");
}

function navigateToSettings() {
  router.push("/settings/advanced");
}

function navigateToNotifications() {
  router.push("/notifications");
}

function navigateToContact() {
  router.push("/contact");
}

function navigateToProfile() {
  router.push("/profile");
}

function navigateToAccount() {
  router.push("/account");
}

function navigateToBilling() {
  router.push("/billings");
}
</script>
<template>
  <SidebarMenu>
    <SidebarMenuItem>
      <DropdownMenu v-model:open="isDropdownOpen">
        <DropdownMenuTrigger as-child>
          <SidebarMenuButton
            size="lg"
            class="data-[state=open]:bg-sidebar-accent data-[state=open]:text-sidebar-accent-foreground"
          >
            <Avatar class="h-8 w-8 rounded-lg">
              <AvatarImage
                :src="
                  authStore.user?.avatar ? String(authStore.user.avatar) : ''
                "
                :alt="
                  authStore.user?.fullname
                    ? String(authStore.user.fullname)
                    : ''
                "
              />
              <AvatarFallback class="rounded-lg"> CN </AvatarFallback>
            </Avatar>
            <div class="grid flex-1 text-left text-sm leading-tight">
              <span class="truncate font-semibold">{{
                authStore.user?.fullname
              }}</span>
              <span class="truncate text-xs">{{ authStore.user?.email }}</span>
            </div>
            <ChevronsUpDown class="ml-auto size-4" />
          </SidebarMenuButton>
        </DropdownMenuTrigger>
        <DropdownMenuContent
          class="w-[--reka-dropdown-menu-trigger-width] min-w-56 rounded-lg"
          :side="isMobile ? 'bottom' : 'right'"
          align="end"
          :side-offset="4"
        >
          <DropdownMenuLabel class="p-0 font-normal">
            <div class="flex items-center gap-2 px-1 py-1.5 text-left text-sm">
              <Avatar class="h-8 w-8 rounded-lg">
                <AvatarImage
                  :src="
                    authStore.user?.avatar ? String(authStore.user.avatar) : ''
                  "
                  :alt="
                    authStore.user?.fullname
                      ? String(authStore.user.fullname)
                      : ''
                  "
                />
                <AvatarFallback class="rounded-lg"> CN </AvatarFallback>
              </Avatar>
              <div class="grid flex-1 text-left text-sm leading-tight">
                <span class="truncate font-semibold">{{
                  authStore.user?.fullname
                }}</span>
                <span class="truncate text-xs">{{
                  authStore.user?.email
                }}</span>
              </div>
            </div>
          </DropdownMenuLabel>
          <DropdownMenuSeparator />

          <DropdownMenuGroup>
            <DropdownMenuItem @click="navigateToProfile">
              <User />
              Profile
            </DropdownMenuItem>
            <!-- <DropdownMenuItem @click="navigateToAccount">
              <Wallet />
              Account
            </DropdownMenuItem> -->
            <DropdownMenuItem
              @click="navigateToBilling"
              v-if="authStore.user?.user_type === 'studio'"
            >
              <CreditCard />
              Billing
            </DropdownMenuItem>
            <!-- <DropdownMenuItem @click="navigateToSettings">
              <Settings />
              Advanced Settings
            </DropdownMenuItem> -->
          </DropdownMenuGroup>
          <DropdownMenuSeparator />
          <DropdownMenuItem
            @click="showLogoutConfirmation"
            class="text-red-500"
          >
            <LogOut />
            Log out
          </DropdownMenuItem>
        </DropdownMenuContent>
      </DropdownMenu>
    </SidebarMenuItem>
  </SidebarMenu>

  <!-- Logout confirmation dialog (outside of dropdown to avoid interaction issues) -->
  <Dialog v-model:open="isLogoutDialogOpen">
    <DialogContent class="sm:max-w-[425px]">
      <DialogHeader>
        <DialogTitle>Confirm Logout</DialogTitle>
        <DialogDescription>
          You're about to sign out of your account. Any unsaved changes may be
          lost. Are you sure you want to continue?
        </DialogDescription>
      </DialogHeader>
      <DialogFooter class="mt-6 flex justify-end gap-3">
        <Button variant="outline" @click="closeLogoutDialog" class="px-5"
          >Cancel</Button
        >
        <Button variant="destructive" @click="handleLogout" class="px-5">
          <LogOut class="mr-2 h-4 w-4" />
          Sign Out
        </Button>
      </DialogFooter>
    </DialogContent>
  </Dialog>
</template>
