---
title: "CSS Animations and the Lightning Design System"
excerpt: >
  This blog discusses CSS animations, and how utilize them with an Aura component and the Lightning Design System.
categories:
  - Salesforce
tags:
  - CSS
  - Lightning Design System
  - Aura
toc: false
---

I've had the fortune of tinkering with Lightning Components and in particular using them with the Lightning Design System lately.  If you're unfamiliar with the Lightning Design System, I strongly urge you to head over to http://www.lightningdesignsystem.com and read up on it now.

There are several goodies and easter eggs in the Lightning Design System, including the "Visibility" utilities (https://www.lightningdesignsystem.com/components/utilities/visibility).  The visibility utilities allow you to show and hide SLDS elements within your application, including in your Lightning Components.

In Lightning Components, you can easily utilize JavaScript controller logic to toggle CSS, utilizing a method like below:

```
({
  hideMe : function(component, event) {
    var el = component.find('myAuraIdGoesHere');
    $A.util.toggleClass(el,'slds-transition-hide');
  }
})
```

Combining the toggleClass method along with the slds-transition-hide class from the Lightning Design System, allows you to hide (and show) elements when some event happens within your lightning component, such as clicking on a close button or perhaps swiping an element left or right in a tindr-like fashion.

The slds-transition-hide class is excellent. However, it has two major issues right out of the gate.

The opacity is set from 0 to 1 immediately, and can be "jarring" to the user.
The opaque / hidden element still takes up space on the screen if it's a block layout, such as a div.
Enter CSS and Styling.  The Visibility documentation on the Lightning Design System hints that you should use a transition property to control transition and animation of the element you're hiding: 

**Note**  To control the timing of the transition, add an additional transition property to control the opacity change.
{: .notice--primary}

The transition property is a CSS style you can give an element to control a limited set of properties such as the opacity and the height of an element.  Unfortunately, the display property is one that you cannot transition on.  In other words, I can't transition from display: inline-block to display: none.  So, to get around the 2nd limitation of the slds-transition-hide styling, we'll have to utilize the height of our element and "shrink" the element over a duration of time.  To do this, I extended two of the classes I'm using in the Lightning Design System by modifying the "Style" of my Lightning Component Bundle:

```
.THIS.slds-media{
  max-height: 250px;
} 

.THIS.slds-transition-hide{
  opacity: 0;
  height: auto;
  max-height: 0;
  transition: opacity 2s linear, max-height 2s linear;
}
```

First, I had to guess the max-height of my div.  The div height is variable, but it *should* never exceed a height of 250px.  Without setting the max height of the visible div, the transition has nothing to compare to the new max-height, so you get a very jarring and immediate transition to 0px if that's the case.  Try it for yourself and you'll see what I mean.

The second class gets toggled when a close button is clicked in my lightning component (from the first snippet above).  Using the height, max-height, and transition properties and applying them to the LDS's slds-transition-hide class, we will now get a fairly smooth transition of both opacity and height over a duration of 2s.

So now that I've showed you both the component bundle's controller and the component bundle's style, let's take a peek at what the component itself looks like:

```
<aura:component >
  <aura:attribute name="someType" type="SomeTypeGoesHere" description="Some Description" />
  <ltng:require styles="/resource/LightningDesignSystem/assets/styles/salesforce-lightning-design-system-vf.css" />
  <div class="slds-media slds-tile slds-hint-parent" aura:id="myElementId">
    <div class="slds-media__figure">
      <img class="img" src="{!v.someVar.Image_Url__c}"/>
    </div>
    <div class="slds-media__body">
      <div class="slds-grid slds-grid--align-spread slds-has-flexi-truncate">
        <p class="slds-tile__title slds-truncate"><a href="#">{!v.someVar.Name}</a></p>
        <button onclick="{!c.hideMe}" class="slds-button slds-button--icon-border-filled slds-button--icon-border-small slds-shrink-none">
          <c:svgIcon class="slds-button__icon slds-button__icon--hint slds-button__icon--small" svgPath="utility-sprite/svg/symbols.svg#close"/>
        </button>
      </div>
      <div class="slds-tile__detail">
        <dl class="dl--horizontal slds-text-body--small">
          <dt class="slds-dl--horizontal__label">
            <p class="slds-truncate">Label:</p>
          </dt>
          <dd class="slds-dl--horizontal__detail slds-tile__meta">
            <p class="slds-truncate">
              {!v.someVar.Value1__c}
            </p>
          </dd>
          <dt class="slds-dl--horizontal__label">
            <p class="slds-truncate">Label2:</p>
          </dt>
          <dd class="slds-dl--horizontal__detail slds-tile__meta">
            <p class="slds-truncate">{!v.someVar.Value2__c}</p>
          </dd>
          <dt class="slds-dl--horizontal__label">
            <p class="slds-truncate">Label3:</p>
          </dt>
          <dd class="slds-dl--horizontal__detail slds-tile__meta">
            <p class="slds-truncate">
              <ui:outputCurrency value="{!v.someVar.Value3__c}" />
            </p>
          </dd>
        </dl>
      </div>
    </div>
  </div>
</aura:component>
```

In the component above, when you click on the close icon, it calls the hideMe method in the controller, which toggles the CSS class starts the CSS transition.  Putting it all together, here's what the component looks like in action:

There you have it, a way to utilize the Lightning Design System, Lightning Components, and CSS Transitions / Animations.  Granted, my solution may not be the best solution.  If you have a better solution than the one above, feel free to comment on this post and let me know.
