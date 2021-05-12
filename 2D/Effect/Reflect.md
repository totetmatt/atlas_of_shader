# Reflect from arbitrary line

```glsl
float angle = mouse.x*3.141592;
vec2 n = vec2(sin(angle),cos(angle)); // Normal of the axe of reflection
/* // The reflection stuff // **/
float d = dot(uv,n); // uv.x*n.x + uv.y*n.y
uv -= n*min(0.,d)*2.; // or max(d,0.);
/***/
col.rg +=sin(uv*10);
col+=smoothstep(0.01,0.,abs(d));
```

### Reference

- https://www.youtube.com/watch?v=il_Qg9AqQkE
