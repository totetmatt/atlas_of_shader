# Concept

To repeate something, the idea is to find a function that will repeat the coordinate system.

Using non continious function like `mod` or `fract` are the most easy way but the counterpart is it might generate artefact.

# Code

Using `uv.x`, but can be replaced by higher dimentional vector.

## Infinite

```
// Using uv.x, but can be expanded to higher dimentional vector

// Simple repetition trick but will have artefact
uv.x = fract(uv.x)-.5;

// Generatlisation
uv.x = mod(uv.x);

// Smooth repetition
uv.x = asin(sin(uv.x)*.9);

// As function
vec2 smoothrepeat_asin_sin(vec2 p,float smooth_size,float size){
    p/=size;
    p=asin(sin(p)*(1.0-smooth_size));
    return p*size;
}

// IQ Generalization
float opRep( in vec3 p, in vec3 c, in sdf3d primitive )
{
    vec3 q = mod(p+0.5*c,c)-0.5*c;
    return primitive( q );
}
```
Triangle sin repetiton
```
vec3 p = asin(sin(p));

// or as explain by wrighter

vec3 p = abs(fract(p)-.5)*4.-1.;
```

## Finite

```
// Simple method
uv.x = abs(uv.x) - .5;

// IQ Generalization
vec3 opRepLim( in vec3 p, in float c, in vec3 l, in sdf3d primitive )
{
    vec3 q = p-c*clamp(round(p/c),-l,l);
    return primitive( q );
}
```

### Reference

- http://mercury.sexy/hg_sdf/
- https://iquilezles.untergrund.net/www/articles/distfunctions/distfunctions.htm
- https://www.youtube.com/watch?v=I8fmkLK1OKg
- https://www.shadertoy.com/view/wlyBWm
