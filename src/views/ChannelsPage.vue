﻿<template>
    <div>
        <v-data-table
            :headers="headers"
            :items="channels"
            :sort-by="['number']"
            class="elevation-1"
        >
            <template v-slot:[`item.actions`]="{ item }">
                <v-btn icon class="mr-2" @click="editRow(item.id)">
                    <v-icon>mdi-lead-pencil</v-icon>
                </v-btn>

                <v-btn icon @click.stop="deleteRow(item.id)">
                    <v-icon>mdi-delete</v-icon></v-btn
                >
            </template>
        </v-data-table>
    </div>
</template>

<script lang="ts">
import { Vue, Component } from 'vue-property-decorator';
import { Channel } from '@/models/Channel';
import { channelApiService } from '@/services/ChannelService';

@Component
export default class Channels extends Vue {
    private channels: Channel[] = [];

    get headers() {
        return [
            { text: this.$t('channels.table.number'), value: 'number' },
            { text: this.$t('channels.table.logo'), value: 'logo' },
            { text: this.$t('channels.table.name'), value: 'name' },
            { text: this.$t('channels.table.language'), value: 'language' },
            { text: this.$t('channels.table.mode'), value: 'streamingMode' },
            {
                text: this.$t('channels.table.ffmpeg-profile'),
                value: 'ffmpegProfile'
            },
            { text: 'Actions', value: 'actions', sortable: false }
        ];
    }

    deleteRow(item: any) {
        let index = this.channels.findIndex((it) => it.id === item.id);
        this.channels.splice(index, 1);
    }

    editRow(item: any) {
        let index = this.channels.findIndex((it) => it.id === item.id);
        this.channels.splice(index, 1);
    }

    title: string = 'Channels';

    async mounted(): Promise<void> {
        this.channels = await channelApiService.getAll();
    }
}
</script>
