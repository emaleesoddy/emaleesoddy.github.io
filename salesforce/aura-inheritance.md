##writing extensible (aura) lightning components through inheritance
<small>originally written & published 18.02.2018</small>

Translating the object-oriented metaphor typically used in compiled programming languages to a descriptive markup language is a bit challenging to wrap your head around, but this is exactly what Lightning Components in Salesforce do. It makes them quite powerful, flexible and minimizes duplicate code &mdash; something that has been trending more and more in front-end web programming.

Thank the gods.

The documentation on how this metaphor is actually implemented in Lightning Components is a little lacking, so this blog post is a brief overview of how it&rsquo;s done.

**Prerequisite:** Understand the basic structure of a Lightning Component (markup, controller, helper).

### Abstract, Extensible, Implements, Interfaces, oh my!
If you&rsquo;re familiar with object-oriented programming, these keywords should sound familiar. For the most part, these keywords mean the same thing when used in a Lightning Component. However, there are some differences, and behavior of these keywords isn&rsquo;t always obvious (or documented).

#### Abstract
Abstract means the same thing it does in traditional OOP. This is a component that doesn't really do anything on its own, and it can&rsquo;t be instantiated on its own. It relies on other components to extend and use its methods. To signify a Lightning Component as abstract, simply add `abstract="true"` to its opening tag.

The cool thing is that if you search for [abstract](https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/oo_abstract_cmp.htm) in Salesforce's documentation, they&rsquo;ll explain it at a high-level, but won&rsquo;t tell you what it actually means for the implementation of your component. If you look at the [attribute list](https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/ref_aura_component.htm) for `aura:component`, they don&rsquo;t list it at all!

So, what does setting the abstract attribute to true actually do?

It means that **you cannot explicitly include the abstract component inside of another component** in code; if you try, that component will fail to save. You can only use an abstract component by extending it with another, non-abstract component.

```language-markup
<aura:component name="My Non-Abstract Component">
    <c:My_Abstract_Component/>
    NOPE. This won't compile.
</aura:component>
```
```language-markup
<aura:component name="My Non-Abstract Component" extends="c:My_Abstract_Component">
    YUP. This is what you should do.
</aura:component>
```

#### Extensible
All abstract components are extensible, even if you don&rsquo;t set the extensible attribute to true, but not all extensible components are abstract. When you extend a component, its child and grand-children will inherit each `aura:attribute` and helper method; there is no reason to define these again in its descendants.

Unlike traditional OOP, you can only extend one component at a time.


```language-markup
<aura:component name="My Component" extends="c:Abstract_Component,c:Base_Component">
    NOPE. This won't compile.
</aura:component>
```

#### Implements & Interfaces
The implements attribute tells the component what [interfaces](https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/oo_interfaces.htm) it can be used in. In the context of Salesforce, this means whether it can be used in the Lightning Experience, a Lightning Community, a custom interface, or any configuration thereof.

I haven&rsquo;t explored writing my own interface, as we generally stick to [the defaults](https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/ref_interfaces.htm) in my line of work, but it&rsquo;s on my list of things to dig into a little deeper.

There does not appear to be a limit to how many interfaces a component can implement, nor does it need to implement an interface at all &mdash; again, both of which make sense in the tradition of OOP. Note that while excluding an interface prevents a user from adding a component to the page from within Salesforce, it does not prevent developers from including it inside another component through code.

### Putting it Together
To illustrate these concepts, I&rsquo;ll be using the metaphors of **vehicles** and **cars**; if you are already familiar with object-oriented principles, this will be review. By putting abstract and extensible components together, your application might look something like this:

* **abstract** Vehicle
  * Car **extends** Vehicle
     * Jeep **extends** Car
     * Pontiac **extends** Car
  * Boat **extends** Vehicle

Translated to Lightning Components and a business use-case, that would look something like:

```language-markup
<aura:component name="Base_Component" abstract="true">
    This component has a lot of helper methods that several other
    components will need to use, like getting a server-side response.
</aura:component>
```
```language-markup
<aura:component name="Feature_Base_Component" extends="Base_Component">
    This component has a lot of helper methods that deal with a single
    object or business requirement. It may or may not be abstract.
</aura>
```
```language-markup
<aura:component name="Visual_Component" extends="Feature_Base_Component">
    This component is a specific piece of UI functionality that
    may need to use helper methods in the Object_Base_Component
    or within the Base_Component! While this component will usually have
    a controller, it generally won't need helper methods of its own.
</aura:component>
```

### Best Practices
* If you have a base component with a lot of methods that other components need to use, but no descriptive markup or value on its own, set the abstract attribute to true.
* If a component can be extended, set the extensible attribute to true (even if it&rsquo;s an abstract).
* If you find yourself writing a helper method twice, put it into a parent component that can be extended by its two children.
* While you can chain extensions near indefinitely, it's usually recommended that you stop after 3-5 extensions.