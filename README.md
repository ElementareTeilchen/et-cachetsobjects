et_cachetsobjects: Caching possibility for expensive TypoScript Objects like menus
==================================================================================

## Idea behind

On bigger pages you have quite often big menus with lots of pages and levels (like responsive menus or flyout menus)
The rendering of these menus is quite heavy and is normally done on every rendered page
If you do not need to mark active or current pages or do this on browser side via Javascript, then the menu might be idential on all / several pages.
Perfect to be cached!

Currently we use it only for menus, other TypoScript objects should work, but probably the cache invalidation needs refinement then

## Usage example

lib.menu.sector = HMENU
lib.menu.sector {
    [...]
}
    // only use caching if no fe_user is logged in, else just stay with original TS
    lib.menu.sector_cached < lib.menu.sector
[loginUser =]
    lib.menu.sector_cached >
    lib.menu.sector_cached = USER
    lib.menu.sector_cached {
        userFunc = ElementareTeilchen\EtCachetsobjects\TypoScriptCache->handleElement
        conf < lib.menu.sector
        cacheTime = 0 // "0" means unlimited liftime, cleared via backend saving hook on page changes
        //here we can set parameter needed for creating different cache entries
        additionalUniqueCacheParameters = COA
        additionalUniqueCacheParameters {
            10 = TEXT
            10.value = 0
            10.override.data = GP:L

            20 = TEXT
            20.value = {$theme.pages.sectorstart_id}
        }
    }
[global]

## Cache Invalidation

In the Extension Manager you can define which variant of invalidation you need.

