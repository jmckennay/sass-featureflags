# sass-featureflags
A simple method for supporting feature flags in SCSS.

> A feature toggle (also feature switch, feature flag, feature flipper, conditional feature, etc.) is a technique in software development that attempts to provide an alternative to maintaining multiple source-code branches (known as feature branches), such that a feature can be tested even before it is completed and ready for release. Feature toggle is used to hide, enable or disable the feature during run time. For example, during the development process, a developer can enable the feature for testing and disable it for other users.
-- [Wikipedia](https://en.wikipedia.org/wiki/Feature_toggle)

So with that in mind I wanted a way to achieve this in SCSS to control the release of styles without adding complicated selectors to the compiled CSS.

View the [demo](https://jmckennay.github.io/scss-featureflags/) page.

# How To

## 1. Include the partial and configure your environments.
At the top of your config partial include the feature partial. After that then define the current environment and the list of environments.
``` SASS
@import "feature.functions";

// Current Environment
$environment: 'ci';

// Environment Names
$environments: (
    'ci',
    'preprod',
    'prod',
);
```

## 2. Configure Feature Flags
Each feature has a list of flags that match the number of environments defined above.
``` SASS
$features: (
    first-feature:  ( true, false, false ),
    second-feature: ( false, false, false )
);
```

## 3. Using a Feature Flag in your SCSS
There are a number of functions and mixins included which allow you to toggle style based on feature flags.
``` SASS
.selector {
    // using the ternary function to control a single property value  
    color: ternary(feature(first-feature), #ffffff, inherit);
    font-weight: t(feature(first-feature), bold, inherit);

    // use the feature mixin
    @include feature(second-feature){
        background: blue;
    }
}

// Use a comma seperated list of features
@include features((first-feature, second-feature)){
    .special-container {
        color: red;
    }
}
```

## Generic Ternary Function
As a way to consume feature flags inline in a single parameter I created a ternary operator function.
``` SASS
// Simple ternary operation
@function ternary($boolean, $if, $else){
    @if ($boolean) { @return $if; } 
    @else { @return $else; }
}

// Short alias of the ternary function
@function t($boolean, $if, $else){
    @return ternary($boolean, $if, $else)
}

// Usage

$flag: true;
$flag2: false;

.selector {
    color: ternary($flag, #ffffff, inherit);
    background: t($flag2, #000000, #111111);
}

// Compiles to
.selector {
    color: #ffffff;
    background: #111111;
}
```