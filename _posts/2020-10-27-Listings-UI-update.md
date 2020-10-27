---
layout: post
title:  "Listings UI Update"
categories: listings
date: 2020-10-27 16:05:00
---

I've added a couple new things to the listings pages, one being the ability to get an individual listing and the other is some UI updates.

I already had the backend setup to return a single listing, so I just had to tell the frontend how to do it.

`api.ts`
```javascript
async getListingByID(id: string) {
return axios.get(`${apiUrl}/api/v1/listings/${id}`, {});
},
```

`actions.ts`
```javascript
async getListingByID(context: MainContext, id: string) {
        try {
            const response = await api.getListingByID(id);
            if (response) {
                return response.data;
            }
        } catch (error) {
            await dispatchCheckApiError(context, error);
        }
    },
...
export const dispatchGetListingByID = dispatch(actions.getListingByID);
```

The individual listings page just makes a call for listing with that ID.

I also changed some anchor tags to buttons:

![](/../assets/2020-10-27-16-09-18.png)

![](/../assets/2020-10-27-16-09-39.png)

![](/../assets/2020-10-27-16-09-56.png)

Not the best looking website, but looking a bit better.
