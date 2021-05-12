# Koch Snowflake

```glsl
//!#[shadertoy]
vec2 uv = (fragCoord.xy -.5* iResolution.xy) / iResolution.y;
vec2 mouse =  iMouse.xy/iResolution.xy;
uv*=1.25;

vec3 col = vec3(0.);

uv.x = abs(uv.x);

uv.y+=tan((5./6.)*3.141592)*.5;

vec2 n = norm((5./6.)*3.141592);
uv -= n*max(0.,dot(uv-vec2(.5,0.),n))*2.;
float scale = 1.;
uv.x +=.5;

n = norm(mouse.x*(2./3.)*3.141592);
for(int i=0;i<=2;i++){
    uv *= 3.;
    scale *=3.;
    uv.x -=1.5;

    uv.x = abs(uv.x);
    uv.x -= .5;
    uv -= n*min(0.,dot(uv,n))*2.;
}
float d = length(uv-vec2(clamp(uv.x,-1.,1.),0));

col += smoothstep(3./iResolution.y,.0,d/scale);
uv/=scale;
col += texture(iChannel0,uv+iTime*.02).rgb;
fragColor = vec4(col,1.0);
```
