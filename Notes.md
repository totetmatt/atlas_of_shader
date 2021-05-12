# Evil 2021-04-13

## Coodr idea

pp.z = mix(pp.z,pp.y+-offset, value);

## Easing

clamp(cos(op.x +tt),-.25,.25)

# Sphere to infinite cylinder to cut

float d = length(p.xz)-r
d = max(d,abs(p.y))

// Compare to actualy accpe cylinder from iq

float sdCappedCylinder( vec3 p, float h, float r )
{
vec2 d = abs(vec2(length(p.xz),p.y)) - vec2(h,r);
return min(max(d.x,d.y),0.0) + length(max(d,0.0));
}

## Evvvil

Global variable are ok to be computed back on normalas long as epsilon is tiny;
