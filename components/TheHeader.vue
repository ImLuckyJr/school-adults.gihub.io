<template>
    <header class="tw-flex tw-flex-row tw-justify-between tw-py-5">
        <div class="header__left tw-flex tw-flex-row">
            <div
                class="
          logo
          2xl:tw-mr-32
          xl:tw-mr-32
          lg:tw-mr-24
          md:tw-mr-24
          sm:tw-mr-16
        "
            >
                <a href="/">
                    <object
                        :data="require('~/assets/images/logo.svg')"
                        style="pointer-events: none"
                        type="image/svg+xml"
                    >
                        <img
                            alt="Логотип"
                            src="~/assets/images/logo.svg"
                        />
                    </object>
                </a>
            </div>
            <div class="links tw-flex tw-flex-row tw-items-stretch tw-gap-9">
                <div
                    :class="{ 'link__item--active': $route.name === 'index' }"
                    class="link__item tw-flex tw-items-center"
                >
                    <NuxtLink
                        v-if="$route.name !== 'index'"
                        to="/"
                    >Главная
                    </NuxtLink>
                    <span v-else>Главная</span>
                </div>
                <div
                    :class="{ 'link__item--active': $route.name === 'courses' }"
                    class="link__item tw-flex tw-items-center"
                >
                    <NuxtLink
                        v-if="$route.name !== 'courses'"
                        to="/courses"
                    >Курсы
                    </NuxtLink>
                    <span v-else>Курсы</span>
                </div>
                <div
                    :class="{ 'link__item--active': $route.name === 'contacts' }"
                    class="link__item tw-flex tw-items-center"
                >
                    <NuxtLink
                        v-if="$route.name !== 'contacts'"
                        to="/contacts"
                    >Контакты
                    </NuxtLink>
                    <span v-else>Контакты</span>
                </div>
                <div
                    :class="{ 'link__item--active': $route.name === 'help' }"
                    class="link__item tw-flex tw-items-center"
                >
                    <a
                        class=""
                        href="#"
                    >Помощь</a>
                </div>
            </div>
        </div>
        
        <div class="header__right tw-flex tw-flex-row">
            <div class="links tw-flex tw-flex-row tw-items-stretch">
                <div
                    v-if="!$auth.loggedIn"
                    :class="{ 'link__item--active': /login|register/.test($route.name) }"
                    class="
            link__item link__item--profile
            tw-flex tw-items-center tw-justify-end
          "
                >
                    <v-menu
                        :nudge-left="50"
                        content-class="header-menu"
                        offset-y
                        right
                    >
                        <template v-slot:activator="{ on, attrs }">
                            <v-btn
                                v-bind="attrs"
                                v-on="on"
                                style="text-transform: capitalize"
                                text
                            >
                                Вход
                            </v-btn>
                        </template>
                        
                        <v-list>
                            <v-list-item
                                v-for="(item, index) in authMenu"
                                :key="index"
                                :to="item.to"
                            >
                                <v-list-item-title style="text-align: right">
                                    {{ item.label }}
                                </v-list-item-title>
                            </v-list-item>
                        </v-list>
                    </v-menu>
                </div>
                <div v-else>
                    <v-menu
                        :nudge-left="25"
                        content-class="header-menu"
                        offset-y
                        right
                    >
                        <template v-slot:activator="{ on, attrs }">
                            <v-badge
                                bordered
                                color="#A8D1E7"
                                icon="mdi-lock"
                                offset-x="20"
                                offset-y="20"
                                overlap
                            >
                                <v-avatar
                                    v-bind="attrs"
                                    v-on="on"
                                    :color="$auth.user.photo ? 'white' : 'red'"
                                    height="85px"
                                    width="85px"
                                >
                                    <img
                                        v-if="$auth.user.photo"
                                        :alt="$auth.user.first_name + ' ' + $auth.user.last_name"
                                        :src="$auth.user.photo"
                                    />
                                    <span
                                        v-else
                                        class="white--text text-h4"
                                    >
                                        {{ getFirstLettersOfName }}
                                    </span>
                                </v-avatar>
                            </v-badge>
                        </template>
                        
                        <v-list>
                            <v-list-item
                                v-for="(item, index) in profileMenu"
                                :key="index"
                                :to="item.to"
                                @click="item.action()"
                            >
                                <v-list-item-title style="text-align: right">
                                    {{ item.label }}
                                </v-list-item-title>
                            </v-list-item>
                        </v-list>
                    </v-menu>
                    <!--<div-->
                    <!--    :nudge-left="50"-->
                    <!--    content-class="header-menu"-->
                    <!--    offset-y-->
                    <!--    right-->
                    <!--&gt;-->
                    <!--    {{ $auth.user.first_name }} {{ $auth.user.last_name }}-->
                    <!--    <img-->
                    <!--        :src="$auth.user.photo"-->
                    <!--        alt=""-->
                    <!--    /><a-->
                    <!--    href="#"-->
                    <!--    @click="$auth.logout()"-->
                    <!--&gt;Выход</a-->
                    <!--&gt;-->
                    <!--</div>-->
                    
                    <div>
                        <!-- {{ $auth.user.first_name }} {{ $auth.user.last_name }} -->
                        <!-- <a href="#" @click="$auth.logout()"
                                      >Выход</a
                                    > -->
                    </div>
                </div>
            </div>
        </div>
    </header>
</template>

<script>
export default {
    name: 'TheHeader',
    data() {
        return {};
    },
    mounted() {
        // Показ загрузки компонента
        this.$nextTick(() => {
            this.$nuxt.$loading.start();
            setTimeout(() => this.$nuxt.$loading.finish(), 500);
        });
    },
    created() {
    },
    computed: {
        authMenu() {
            return [
                {
                    to:    'login',
                    label: 'Вход',
                },
                {
                    to:    'register',
                    label: 'Регистрация',
                },
            ];
        },
        
        profileMenu() {
            return [
                {
                    to:     undefined,
                    label:  'Выход',
                    action: this.logout,
                },
            ];
        },
        
        getFirstLettersOfName() {
            return (
                this?.$auth?.user?.first_name.slice(0, 1).toUpperCase() +
                this?.$auth?.user?.last_name.slice(0, 1).toUpperCase()
            );
        },
    },
    methods:  {
        async logout() {
            await this.$auth.logout();
        },
    },
};
</script>

<style lang="scss">
header {
    font-size: 1.5rem;
    
    .link {
        $self: &;
        
        &__item {
            text-decoration: none !important;
            
            a {
                text-decoration: none !important;
            }
            
            > span {
                cursor: pointer;
            }
            
            &--active {
                > span,
                > a {
                    color: #f76d6d;
                }
                
                #{$self}--profile {
                    .dropdown {
                        color: #f76d6d !important;
                        
                        &::before {
                            background-image: url("~/assets/images/user-icon-hover.svg");
                        }
                    }
                }
            }
            
            &:hover,
            li:hover {
                > span,
                > a {
                    color: #f76d6d !important;
                }
            }
            
            &--profile {
                .v-btn {
                    color:          inherit !important;
                    font-size:      1.5rem !important;
                    text-indent:    0 !important;
                    letter-spacing: normal !important;
                    transition:     none !important;
                    
                    &.btn {
                        transition: none !important;
                    }
                    
                    &::after {
                        display: none !important;
                    }
                    
                    &:hover {
                        background-color: transparent !important;
                        color:            #f76d6d !important;
                        
                        &::before {
                            background-image: url("~/assets/images/user-icon-hover.svg");
                        }
                    }
                    
                    &::before {
                        position:         relative;
                        display:          inline-block;
                        content:          " ";
                        background-image: url("~/assets/images/user-icon.svg");
                        background-size:  30px 36px;
                        width:            30px;
                        height:           36px;
                        margin-right:     0.5rem;
                        opacity:          1 !important;
                        background-color: transparent !important;
                    }
                    
                    &__content {
                        font-weight: normal !important;
                    }
                }
                
                li {
                    text-align: right;
                    
                    &:hover a {
                        background-color: transparent;
                    }
                }
            }
        }
    }
}

.header-menu {
    box-shadow:    none !important;
    border:        3px solid #a8d1e7 !important;
    border-radius: 10px !important;
    margin-top:    0.2rem;
    
    .v-list {
        margin-top:     0 !important;
        padding-top:    0 !important;
        margin-bottom:  0 !important;
        padding-bottom: 0 !important;
        
        &:hover {
            background-color: transparent;
        }
        
        &-item {
            &--active,
            &:hover {
                color: #f76d6d !important;
                
                &:before {
                    opacity: 0;
                }
            }
            
            &__title {
                font-size:   24px;
                line-height: 32px;
                
                &:hover {
                    background-color: transparent;
                    color:            #f76d6d !important;
                }
            }
        }
    }
}
</style>