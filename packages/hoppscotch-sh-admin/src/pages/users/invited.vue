<template>
  <div>
    <div class="ml-3">
      <button
        class="p-2 mb-2 rounded-3xl bg-zinc-800"
        @click="router.push('/users')"
      >
        <icon-lucide-arrow-left class="text-xl" />
      </button>
    </div>
    <h3 class="sm:px-6 p-4 text-3xl font-bold text-gray-200 mb-2">
      Invited Users
    </h3>

    <div class="flex flex-col">
      <div class="py-2 -my-2 overflow-x-auto sm:-mx-6 sm:px-4 lg:-mx-8 lg:px-8">
        <div class="inline-block min-w-full overflow-hidden align-middle">
          <div class="sm:px-7 p-4">
            <div>
              <div v-if="fetching" class="flex justify-center">
                <HoppSmartSpinner />
              </div>
              <div v-else-if="error">
                <p class="text-xl">No Invited Users Found..</p>
              </div>

              <table v-else class="w-full text-left">
                <thead>
                  <tr
                    class="text-gray-200 border-b border-gray-600 text-sm font-bold"
                  >
                    <th class="px-3 pt-0 pb-3">Admin ID</th>
                    <th class="px-3 pt-0 pb-3">Admin Email</th>
                    <th class="px-3 pt-0 pb-3">Invitee Email</th>
                    <th class="px-3 pt-0 pb-3">Invited On</th>
                  </tr>
                </thead>
                <tbody
                  v-for="user in invitedUsers"
                  id="user.id"
                  class="text-gray-300"
                >
                  <tr
                    class="border-b border-gray-600 hover:bg-zinc-800 rounded-xl"
                  >
                    <td class="sm:p-3 py-2 px-3 max-w-30 ml-2">
                      <div>
                        <span class="truncate">
                          {{ user?.adminUid }}
                        </span>
                      </div>
                    </td>
                    <td class="sm:p-3 py-2 px-1">
                      <span class="flex items-center ml-2">
                        {{ user?.adminEmail }}
                      </span>
                    </td>
                    <td class="sm:p-3 py-2 px-1 ml-4">
                      <span class="ml-2">
                        {{ user?.inviteeEmail }}
                      </span>
                    </td>

                    <td class="sm:p-3 py-2 px-1">
                      <div class="flex items-center ml-2">
                        <div class="flex flex-col">
                          {{ getCreatedDate(user?.invitedOn) }}
                          <div class="text-gray-400 text-xs">
                            {{ getCreatedTime(user?.invitedOn) }}
                          </div>
                        </div>
                      </div>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
import { useQuery } from '@urql/vue';
import { InvitedUsersDocument } from '../../helpers/backend/graphql';
import { format } from 'date-fns';
import { HoppSmartSpinner } from '@hoppscotch/ui';
import { useRouter } from 'vue-router';

const router = useRouter();

// Get Proper Date Formats
const getCreatedDate = (date: string) => format(new Date(date), 'dd-MM-yyyy');
const getCreatedTime = (date: string) => format(new Date(date), 'hh:mm a');

// Get Invited Users
const { fetching, error, data } = useQuery({ query: InvitedUsersDocument });
const invitedUsers = computed(() => data?.value?.admin.invitedUsers);
</script>