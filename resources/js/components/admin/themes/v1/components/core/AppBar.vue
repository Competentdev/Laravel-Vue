<template>
    <v-app-bar
            id="core-app-bar"
            app
            flat
            clipped-left
            height="70"
            hide-on-scroll
            elevate-on-scroll
            fixed
            style="position: relative !important;"
    >
        <v-toolbar-title class="tertiary--text font-weight-light align-self-center">
            <v-btn
                    v-if="responsive"
                    dark
                    icon
                    @click.stop="onClick"
            >
                <v-icon>mdi-view-list</v-icon>
            </v-btn>
            <img src="../../../../assets/img/logo.png" alt="" height="70">
        </v-toolbar-title>

        <v-spacer/>

        <v-text-field
                app
                class="purple-input"
                label="Searching..."
                hide-details
                color="purple"
                prepend-inner-icon="search"
        ></v-text-field>
        <v-spacer/>

        <v-toolbar-items>
            <v-row
                    align="center"
                    class="mx-2"
            >

                <v-menu
                        bottom
                        left
                        offset-y
                        transition="slide-y-transition"
                        mr-3
                >
                    <template v-slot:activator="{ attrs, on }">
                        <!--<v-btn v-on="on">-->
                        <div v-on="on"
                             class="mr-4">
                            <v-avatar size="36">
                                <img src="../../assets/img/faces/face-1.jpg">
                            </v-avatar>
                            Simon Groger
                        </div>
                        <!--</v-btn>-->
                    </template>

                    <v-card>
                        <v-list dense>
                            <v-list-item>
                                <v-list-item-content>
                                    <v-list-item-title>My Profile</v-list-item-title>
                                </v-list-item-content>
                            </v-list-item>
                            <v-list-item>
                                <v-list-item-content>
                                    <v-list-item-title>LogOut</v-list-item-title>
                                </v-list-item-content>
                            </v-list-item>
                        </v-list>
                    </v-card>
                </v-menu>

                <v-menu
                        bottom
                        left
                        offset-y
                        transition="slide-y-transition"
                >
                    <template v-slot:activator="{ attrs, on }">
                        <v-btn
                                class="toolbar-items mr-4"
                                icon
                                v-bind="attrs"
                                v-on="on"
                        >
                            <v-badge
                                    color="error"
                                    overlap
                            >
                                <template slot="badge">
                                    {{ notifications.length }}
                                </template>
                                <v-icon>
                                    mdi-bell
                                </v-icon>
                            </v-badge>
                        </v-btn>
                    </template>

                    <v-card>
                        <v-list dense>
                            <v-list-item
                                    v-for="notification in notifications"
                                    :key="notification"
                                    @click="onClick"
                            >
                                <v-list-item-title v-text="notification"/>
                            </v-list-item>
                        </v-list>
                    </v-card>
                </v-menu>

                <v-btn
                        icon
                        @click.stop="toggleMenu"
                >
                    <v-icon >
                        mdi-view-list
                    </v-icon>
                </v-btn>
            </v-row>
        </v-toolbar-items>
    </v-app-bar>
</template>

<script>
    // Utilities
    import {
        mapMutations
    } from 'vuex'

    export default {
        data: () => ({
            notifications: [
                'Mike, John responded to your email',
                'You have 5 new tasks',
                'You\'re now a friend with Andrew',
                'Another Notification',
                'Another One'
            ],
            title: null,
            responsive: false
        }),

        watch: {
            '$route'(val) {
                this.title = val.name
            }
        },

        mounted() {
            this.onResponsiveInverted()
            window.addEventListener('resize', this.onResponsiveInverted)
        },
        beforeDestroy() {
            window.removeEventListener('resize', this.onResponsiveInverted)
        },

        methods: {
            ...mapMutations('app', ['setDrawer', 'toggleDrawer']),
            onClick() {
                this.setDrawer(!this.$store.state.app.drawer)
            },
            onResponsiveInverted() {
                if (window.innerWidth < 991) {
                    this.responsive = true
                } else {
                    this.responsive = false
                }
            },
            toggleMenu() {
                this.$eventHub.$emit('toggleMenuV1');
            },
        }
    }
</script>

<style>
    /* Fix coming in v2.0.8 */
    #core-app-bar {
        width: auto;
    }

    #core-app-bar a {
        text-decoration: none;
    }
</style>
