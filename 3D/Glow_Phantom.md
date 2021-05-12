```glsl bonzomatic

float sdf(vec3 p){
   vec3 pp = p;
  vec3 id = floor(p);
  p.y+=2.;
  p.xz = mod(p.xz+.5*2.,2.)-.5*2.;
  p.y+=cos(pp.x*.2-fGlobalTime)+sin(pp.z*.2+fGlobalTime);
  return .9*(length(p)-1.);
}

vec2 nv=vec2(-.0001,.0001);
#define q(s) s*sdf(p+s)
vec3 norm(vec3 p){
  return normalize(q(p.xyy) + q(p.yyx) +q(p.yxy) +q(p.xxx)  );
}
void main(void)
{
	vec2 uv = vec2(gl_FragCoord.x / v2Resolution.x, gl_FragCoord.y / v2Resolution.y);
	uv -= 0.5;
	uv /= vec2(v2Resolution.y / v2Resolution.x, 1);

	vec3 col = vec3(.1);
  vec3 ro=vec3(0.,0.,-5.),rd=normalize(vec3(uv,1.)),rp=ro;
  vec3 light=vec3(1.,2.,-3.);

  float acc=0.;
  for(float i=0.;i<=69.;i++){
      float d = sdf(rp);
      d= max(0.02,abs(d));
      acc+=exp(5.*-abs(d))/69.;
      
      rp+=rd*d;
  }
  col +=acc;
	out_color = vec4(col,1.);
}
```
