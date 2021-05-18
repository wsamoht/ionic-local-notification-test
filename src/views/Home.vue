<template>
    <ion-page>
        <ion-header :translucent="true">
            <ion-toolbar>
                <ion-title>Local Notification Test</ion-title>
            </ion-toolbar>
        </ion-header>

        <ion-content :fullscreen="true">
            <div class="ion-margin-horizontal">
                <ion-button expand="block" @click="addNotification">
                    Add notification
                </ion-button>
                <p>Pending notifications:</p>
                <pre style="white-space: pre-wrap">
                    {{ notifications }}
                </pre>
            </div>
        </ion-content>
    </ion-page>
</template>

<script lang="ts">
import {
    IonButton,
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
} from "@ionic/vue";
import { defineComponent } from "vue";
import { LocalNotifications } from "@capacitor/local-notifications";

export default defineComponent({
    name: "Home",
    components: {
        IonButton,
        IonContent,
        IonHeader,
        IonPage,
        IonTitle,
        IonToolbar,
    },
    data() {
        return {
            notifications: "",
        };
    },
    methods: {
        async addNotification(): Promise<void> {
            await LocalNotifications.schedule({
                notifications: [
                    {
                        id: new Date().getTime(),
                        title: "My test title",
                        body: "My test text",
                        schedule: {
                            on: {
                                hour: 12,
                                minute: 30,
                            },
                            allowWhileIdle: true,
                        },
                    },
                ],
            });

            const pending = await LocalNotifications.getPending();
            this.notifications = JSON.stringify(pending.notifications);
            console.log(pending.notifications);
        },
    },
});
</script>