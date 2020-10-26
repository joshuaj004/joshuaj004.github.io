---
layout: post
title:  "Listings Update"
categories: listings
date: 2020-10-26 17:53:00
---

I've made a couple updates to the listings! The first is that the backend can now query all of the listings from the DB (with a default max of 100).

```python
@router.get("/")
def get_listings(
    db: Session = Depends(deps.get_db)
) -> Any:
    """
    Retrieve all listings, with a maximum of 100
    """
    listings = crud.listing.get_listings(
        db=db, skip=0, limit=100
    )
    return listings  
```

I also updated the frontend to make a call to this instead of returning dummy data. 

It looks like this:

```javascript
import { dispatchCheckApiError } from '../main/actions';

type MainContext = ActionContext<ListingState, State>;

export const actions = {
    async getListings(context: MainContext) {
        try {
            const response = await api.getListings();
            if (response) {
                return response.data;
            }
        } catch (error) {
            await dispatchCheckApiError(context, error);
        }
    },
};
```

I made a couple minor CSS changes and added a default image placeholder of some kittens.

![](/../assets/2020-10-26-18-04-22.png)