---
title: getting components of different types with @ViewChild
level: advanced
author: thekiba
tags:
  - good-to-know
  - tips
  - components
  - dependency-injection
---

# Content
It's possible to use @ViewChild to query for components of different types using dependency injection. 

In the example below we can use `@ViewChildren(Base)` to get instances of `Foo` and `Bar`.

This also applies for @ViewChildren and @ContentChild/Children.

```typescript
abstract class Base {}

@Component({
  providers: [{ provide: Base, useExisting: Foo }]
})
class Foo extends Base {}

@Component({
  providers: [{ provide: Base, useExisting: Bar }]
})
class Bar extends Base {}
```

```typescript
@ViewChildren(Base) components: QueryList<Base>;
```

# Links
https://www.youtube.com/watch?v=PRRgo6F0cjs

# ComponentCode
import { Component, ViewChildren, QueryList, AfterViewInit } from '@angular/core';

export abstract class Animal {
    abstract say();
}

@Component({
  selector: 'fox',
  template: `🦊`,
  providers: [{ provide: Animal, useExisting: Fox }]
})
export class Fox extends Animal {
    say() {
        console.log('Joff-tchoff-tchoffo-tchoffo-tchoff!');
    }
}

@Component({
  selector: 'rice',
  template: `🍚`,
  providers: [{ provide: Animal, useExisting: Rice }]
})
export class Rice extends Animal {
    say() {
        console.log('lol');
    }
}

@Component({
  selector: 'dragon',
  template: `🐉`,
  providers: [{ provide: Animal, useExisting: Dragon }]
})
export class Dragon extends Animal {
    say() {
        console.log('Wa-pa-pa-pa-pa-pa-pow!');
    }
}

@Component({
    selector: 'my-app',
    template: `
        <fox></fox>
        <rice></rice>
        <dragon></dragon>
    `
})
export class AppComponent implements AfterViewInit {
    @ViewChildren(Animal) animals: QueryList<Animal>;

    ngAfterViewInit() {
        animals.forEach((animal) => animal.say());
    }
}

# ModuleCode
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent, Fox, Rice, Dragon } from './app.component';

@NgModule({
  imports: [BrowserModule],
  declarations: [AppComponent, Fox, Rice, Dragon],
  bootstrap: [AppComponent]
})
export class AppModule {}