---
layout: post
title:  "Getting Fake Listings"
categories: api frontend
date: 2020-10-24 14:40:00
---

I have been so busy lately that I haven't had time to sit down and continue working on this api project. So that you don't have to dig back into my posts, basically I was trying to make a craigslist mvp and I was working on the frontend portion, but I was running into an issue where I couldn't pull fake data. The solution came down to something so simple. I had created a listings folder with various actions, state, getters, etc. files but I had forgotten to export the module and import the listings module into the store. This meant that my listings vue components could not find the listing getter functions. This will probably make more sense when I show the code. 

`Listings.vue`

```typescript
<template>
  <v-content>
    <v-container fluid fill-height>
      <v-layout align-center justify-center>
        <v-flex xs12 sm8 md4>
          <h1>THIS IS THE LISTINGS PAGE</h1>
          <ol>
            <li 
              v-for="listing in listings"
              v-bind="listing" 
              v-bind:key="listing.id"
            >{{ listing.title }}</li>
          </ol>
          <h1>
            <a href='newListing'>CLICK HERE TO MAKE A NEW LISTING</a>
          </h1>
        </v-flex>
      </v-layout>
    </v-container>
  </v-content>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import { api } from '@/api';
import { appName } from '@/env';
import { component } from 'vue/types/umd';
import { dispatchGetListings } from '@/store/listings/actions';

@Component
export default class Listings extends Vue {
  public appName = appName;

  public listings: any[] = [];

  private beforeMount() {
    dispatchGetListings(this.$store)
    .then((listings) => {
      this.listings = listings;
    });
  }
}

</script>
```

`actions.ts`

```typescript
import { api } from '@/api';
import { ActionContext } from 'vuex';
import { State } from '../state';
import { ListingState } from './state';
import { getStoreAccessors } from 'typesafe-vuex';

type MainContext = ActionContext<ListingState, State>;

export const actions = {
    async getListings(context: MainContext) {
        return [
            { id: 0, title: 'For Sale: Shoes, never worn.'},
            { id: 1, title: 'For Sale: Shoes, hardly worn.'},
            { id: 2, title: 'For Sale: Shoes, worn.'},
            { id: 3, title: 'For Sale: Shoes, definitely worn.'},
            { id: 4, title: 'For Sale: Shoes, heavily worn.'},
          ];
    },
};

const { dispatch } = getStoreAccessors<ListingState, State>('');

export const dispatchGetListings = dispatch(actions.getListings);
```

As you can see, this `getListings` function just returns some dummy data that is rendered out on the Listings page.

I added empty `state.ts`, `mutations`, and `getters.ts` files. I also exported a `listingModule` from the `index.ts` file.

`index.ts`

```typescript
import { mutations } from './mutations';
import { getters } from './getters';
import { actions } from './actions';
import { ListingState } from './state';

const defaultState: ListingState = {
  users: [],
};

export const listingModule = {
  state: defaultState,
  mutations,
  actions,
  getters,
};
```

This was imported in the `index.ts` file one level up.

```typescript
import Vue from 'vue';
import Vuex, { StoreOptions } from 'vuex';

import { mainModule } from './main';
import { State } from './state';
import { adminModule } from './admin';
import { listingModule } from './listings';

Vue.use(Vuex);

const storeOptions: StoreOptions<State> = {
  modules: {
    main: mainModule,
    admin: adminModule,
    listings: listingModule,
  },
};

export const store = new Vuex.Store<State>(storeOptions);

export default store;
```

It's not pretty, but it works for now. 

![](/../assets/2020-10-24-14-48-56.png)

Up next, I'd either want to grab real data from the DB or create a new listings page with a form so that new listings can be inserted into the database. 