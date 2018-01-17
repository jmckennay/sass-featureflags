# scss-featureflags
A simple method for supporting feature flags in SCSS


## Configure Environment and Environment Names
``` SASS
// Current Environment
$environment: 'ci';

// Environment Names
$environments: (
    'ci',
    'preprod',
    'prod',
);
```

## Configure Feature Flags
Each feature has a list of flags that match the number of environments defined above
``` SASS
$features: (
    first-feature:  ( true, false, false ),
    second-feature: ( false, false, false )
);
```

## Using a Feature Flag in your SCSS
There are a number of functions and mixins included which allow you to toggle style based on feature flags.
``` SASS

.selector {
    // using the ternary function to control a value  
    color: ternary(feature(second-feature), #ffffff, inherit);
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