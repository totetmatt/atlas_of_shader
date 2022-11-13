## Default function found on the web
Based on https://thebookofshaders.com/10/
also first result on google for "random glsl"
https://stackoverflow.com/questions/4200224/random-noise-functions-for-glsl
also defined as fsnoise in [twigl.app](https://github.com/doxas/twigl)
```c
float rand(vec2 co){
    return fract(sin(dot(co, vec2(12.9898, 78.233))) * 43758.5453);
}
```
## Random unit vector
https://suricrasia.online/demoscene/functions/#rndunit

https://suricrasia.online/demoscene/functions/#hash
