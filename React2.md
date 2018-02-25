# Structure components

![IMG](https://media.giphy.com/media/OMeGDxdAsMPzW/giphy.gif)

# Why?

> In order to keep things modular, separating concerns and build complex views. We may end up with a big "bicho". So that's why nested components comes to help us structuring the relationship between them.

# Ways

1. A composes B and B composes C

```html
<app>   <!-- A -->
  <GifList />  <!-- B -->
  <GifForm />
</app>

<!-- Separate file -->
<GifList>  
 <titleList />  <!-- C -->
</GifList>
<!--  -->
```

> Pros

* Easy and fast to separate UI elements
* Easy to inject props down to children based on the parent component's state

> Cons

* Less visibility into the composition architecture
* Less reusability

> When should I use it?

* B and C are just presentational components
* B should be responsible for C's lifecycle

2. A composes B and A tells B to compose C.

```html
<app>   <!-- A -->
  <GifList>  <!-- B -->
    <titleList />  <!-- C -->
  </GifList>
  <GifForm />
</app>
```

> Pros

* Better components lifecycle management
* Better visibility into the composition architecture
* Better reusuability

> Cons

* Injecting props can become a little expensive
* Less flexibility and power in child components
* B doesn't know what the children are for.

> When should I use it?

* B should accept to compose something different than C in the future or somewhere else
* A should control the lifecycle of C

3. A composes B and B provides an option for A to pass something to compose for a specific purpose.

```html
<app>   <!-- A -->
  <GifList title={titleList}>  <!-- B -->
  </GifList>
  <GifForm />
</app>
```

> Pros

* Composition as a feature
* Easy validation
* Better composaiblility

> Cons

* Injecting props can become a little expensive
* Less flexibility and power in child components

> When should I use it?

* B has specific features defined to compose something
* B should only know how to render not what to render

[Continue 3](React3.md)
