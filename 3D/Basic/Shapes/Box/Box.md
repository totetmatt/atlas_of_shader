```glsl shadertoy
#define MOVE_LIGHT false
#define MOVE_OBJECT true

mat2 rotate(float angle){
    return mat2(cos(angle),-sin(angle),sin(angle),cos(angle));
}

float box(vec3 position, vec3 boxSize){
    vec3 a = abs(position) - boxSize;
    return length(max(vec3(0.),a)) + min(max(a.x,max(a.y,a.z)),0.);
}

float sdf(vec3 position){

    if(MOVE_OBJECT){
        position.xz*=rotate(iTime);
    }

    float dist = box(position,vec3(2.,.5,.5));
    return dist;
}

#define normSwizzle(e) e*sdf(p+e)
vec2 normVecHelper=vec2(-0.0001,0.0001);
vec3 normVec(vec3 p){
    return normalize(
    normSwizzle(normVecHelper.xyy) +
    normSwizzle(normVecHelper.yxy) +
    normSwizzle(normVecHelper.yyx) +
    normSwizzle(normVecHelper.xxx)
    );
}
void mainImage(out vec4 fragColor, in vec2 fragCoord)
{
    vec2 uv = (fragCoord.xy -.5* iResolution.xy)/iResolution.y;

    vec3 color = vec3(.1);

    vec3 rayOrigin    = vec3(0.,0.,-7),
         rayDirection = normalize(vec3(uv,1.)),
         rayPosition  = rayOrigin;

    vec3 lightPosition= vec3(1.,2.,-3);

    if(MOVE_LIGHT){
        lightPosition = vec3(cos(iTime*.33)*3.,2.,sin(iTime*.33)*3.);
    }

    const float LIMIT_RAY_MARCH = 69.;
    for(float i=0.;i<=LIMIT_RAY_MARCH;i++){
        float dist = sdf(rayPosition);
        if(dist <=0.0001) {
            vec3 normal = normVec(rayPosition);
            float light = max(0.,dot(normalize(lightPosition),normal));
            light *= (1.-(i/LIMIT_RAY_MARCH));

            color = vec3(1.)*light;
            break;
        }
        rayPosition += dist * rayDirection;
    }

    fragColor = vec4(color,1.0);
}
```
