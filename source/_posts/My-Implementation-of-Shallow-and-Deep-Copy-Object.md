---
title: Implementation of Object's Shallow and Deep Copy 
date: 2016-06-24 20:13:18
tags: javascript
---

Today, someone asked me how to do implement object's shallow copy and deep copy. I told him the idea without code. Now I'm trying to write it down and add unit test for it.

Let's say, the API will be like this `copy(src, dest, deep)`, nothing returned.
If `deep` is true, `src` will be deep copied to `dest`

OK, let's write the unit test first to make sure we are at same page.

```
    it('can shallow copy an object', function() {
        var src = {
            'name': 'zzsanduo',
            'info': {
                'hobby': 'cycling'
            },
            'experience': [ 'BUPT' ]
        };
        var dest = {};

        copy(src, dest);
        src.name = 'ZHAO Yan';
        src.info.hobby = 'singing';
        src.experience.push('future');

        expect(dest.name).toBe('zzsanduo');
        expect(dest.info.hobby).toBe('singing');
        expect(dest.experience.length).toBe(2);
    });
```
So without any code yet, the test will fail. Now let's add the code.


```
function copy(src, dest, deep) {
    if (!src || !dest || typeof src !== "object" || typeof dest !== "object") {
        return;
    }
    for (var p in src) {
        dest[p] = src[p];
        if (deep && Array.isArray(src[p])) {
            dest[p] = [];
            deepCopyArray(src[p], dest[p])
        } else if (deep && typeof src[p] === "object") {
            dest[p] = {};
            copy(src[p], dest[p], deep);
        }
    }
}

function deepCopyArray(src, dest) {
    if (!dest || !src || !Array.isArray(src) || !Array.isArray(dest)) {
        return;
    }
    for (var i in src) {
        dest[i] = src[i];
        if (Array.isArray(src[i])) {
            dest[i] = [];
            deepCopyArray(src[i], dest[i]);
        } else if (typeof src[i] === "object") {
            dest[i] = {};
            copy(src[i], dest[i], true);
        }
    }
}
```
The code is my own implementation,not perfect one, which can be improved definitly. 

You can find code at [here](https://github.com/zzsanduo/javascript-wheels). If you have better idea, please join me to improve it!
