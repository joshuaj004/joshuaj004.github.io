---
layout: post
title:  "Initial Work on the Listings MVP"
categories: frontend backend database
date: 2020-10-14 15:20:00
---

Yesterday I was complaining about buying stuff online and I started working on an MVP. There were a lot of new things that I have to learn and a lot of moving parts that I had to touch to even start. I'll separate them into frontend and backend. 


## Frontend
I started by making a listings page with dummy data that shows all the current listings.  

Listings page

![](/../assets/2020-10-14-14-59-45.png)

You can click on a listing to take you to the page for that listing.

![](/../assets/2020-10-14-15-00-07.png)

Inside the listings page, I created a small Listing component to handle linking as well as displaying the title. 

```javascript
Vue.component('Listing', {
  props: ['title', 'id', 'url'],
  template: `
  <a :href="'listings/' + id">
    <div>
      <h1>{{ id }} - {{ title }}</h1>
    </div>
  </a>`,
});
```

In order to actually be able to use this component inside other components, I had to add this line to `vue.config.js`
```javascript
runtimeCompiler: true,
```

I also added the routings to the `router.ts` file for both the Listings page and the individual Listing pages.
```javascript
{
    path: 'listings',
    component: () => import('./views/Listings.vue'),
},
{
    path: 'listings/:id',
    component: () => import('./views/Listing.vue'),
},
```

As you can see, the frontend is still using dummy data and it doesn't look very pretty. For now, the heavy lifting will be done on the backend.

## Backend

I'm not really sure how to order this, I think it makes sense to start with the DB related changes and then move to the API changes.

### Database

In the models folder, I created a Listing class:
```python
from typing import TYPE_CHECKING

from sqlalchemy import Boolean, Column, Integer, String
from sqlalchemy.orm import relationship

from app.db.base_class import Base

# if TYPE_CHECKING:
#     from .item import Item  # noqa: F401


class Listing(Base):
    id = Column(Integer, primary_key=True, index=True)
    title = Column(String, index=True)
    description = Column(String)
```

Which was then imported into the `__init.py__` file:
```python
from .listing import Listing
```

The models also have to import into the `base.py` file so that Alembic can import them. 

From here, I followed the instructions for database migrations from the README. This involved starting an interactive session into the backend container and autogenerating an alembic revision.

### Schemas

I added a very basic schema for Listings, just a title and a description
```python
from typing import Optional

from pydantic import BaseModel


# Shared properties
class ListingBase(BaseModel):
    title: Optional[str] = None
    description: Optional[str] = None


# Properties to receive on item creation
class ListingCreate(ListingBase):
    title: str
    description: str


# Properties to receive on item update
class ListingUpdate(ListingBase):
    pass


# Properties shared by models stored in DB
class ListingInDBBase(ListingBase):
    id: int
    title: str

    class Config:
        orm_mode = True


# Properties to return to client
class Listing(ListingInDBBase):
    pass


# Properties properties stored in DB
class ListingInDB(ListingInDBBase):
    pass
```

This schema was imported into the `__init__.py` file for the schemas.
```python
from .listing import Listing, ListingCreate, ListingInDB, ListingUpdate
```

### CRUD
Right now there are only 2 methods for the Listings that interact with the database, create_listing and get_listing_by_id. 
```python
from typing import List

from fastapi.encoders import jsonable_encoder
from sqlalchemy.orm import Session

from app.crud.base import CRUDBase
from app.models.listing import Listing
from app.schemas.listing import ListingCreate, ListingUpdate


class CRUDListing(CRUDBase[Listing, ListingCreate, ListingUpdate]):
    def create_listing(
        self, db: Session, *, obj_in: ListingCreate
    ) -> Listing:
        obj_in_data = jsonable_encoder(obj_in)
        db_obj = self.model(**obj_in_data)
        db.add(db_obj)
        db.commit()
        db.refresh(db_obj)
        return db_obj

    def get_listing_by_id(
        self, db: Session, *, id: int, skip: int = 0, limit: int = 100
    ) -> List[Listing]:
        return (
            db.query(self.model)
            .filter(Listing.id == id)
            .offset(skip)
            .limit(limit)
            .all()
        )


listing = CRUDListing(Listing)
```

This is also imported into the crud `__init__.py` file.

```python
from .crud_listing import listing
```

### API 
Finally we're down to the last portion - the API portion. 

This handles the 2 methods described previously.
```python
from fastapi import APIRouter, Depends, HTTPException
from sqlalchemy.orm import Session
from typing import Optional, Any, List

# from app import schemas
from app import crud, models, schemas
from app.api import deps

router = APIRouter()

@router.get("/{listing_id}")
def get_listing(
    listing_id: int,
    db: Session = Depends(deps.get_db)
) -> Any:
    """
    Retrieve a specific listing
    """
    listings = crud.listing.get_listing_by_id(
        db=db, id=listing_id, skip=0, limit=100
    )
    return listings   

@router.post("/")
def create_listing(
    *,
    db: Session = Depends(deps.get_db),
    listing_in: schemas.ListingCreate,
) -> Any:
    """
    Create new listing.
    """
    listing = crud.listing.create_listing(db=db, obj_in=listing_in)
    return listing
```

And of course this has to be imported to, in this case to the `api.py` file with these two lines

```python
from app.api.api_v1.endpoints import items, login, users, utils, sudoku, listings # The addition of listings
...
api_router.include_router(listings.router, prefix="/listings", tags=["listings"])
```

Ignoring the frontend for a second, what this gives us is a way to create a simple listing (title and description), store it in a database, and retrieve a listing by ID (autogenerated and autoincrementing). When I write this out, it both seems too simple and too much at the same time. I'm chalking that up to the fact that I had to figure out all these moving parts! If I were to redo this, it should be a lot faster! I'm trying to decide if my next step should be to pull in listings and display them on the frontend or to bolster the backend. Who knows? I'll probably decide tomorrow!