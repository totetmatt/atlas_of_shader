```glsl bonzomatic
#version 410 core

uniform float fGlobalTime; // in seconds
uniform vec2 v2Resolution; // viewport resolution (in pixels)
uniform float fFrameTime; // duration of the last frame, in seconds

uniform sampler1D texFFT; // towards 0.0 is bass / lower freq, towards 1.0 is higher / treble freq
uniform sampler1D texFFTSmoothed; // this one has longer falloff and less harsh transients
uniform sampler1D texFFTIntegrated; // this is continually increasing
uniform sampler2D texPreviousFrame; // screenshot of the previous frame
uniform sampler2D texChecker;
uniform sampler2D texNoise;
uniform sampler2D texTex1;
uniform sampler2D texTex2;
uniform sampler2D texTex3;
uniform sampler2D texTex4;


layout(location = 0) out vec4 out_color; // out_color must be written in order to see anything
vec2 z,v,e=vec2(.00035,-.00035);float t,tt,b,bb,g,gg,a,la,wa;vec3 np,pp,cp,po,no,al,ld;
vec4 c=vec4(0,5,10,.2);
vec2 mp( vec3 p)
{
  vec2 h,t=vec2(length(p)-2,5);
  return t;
}
vec2 tr( vec3 ro,vec3 rd)
{
  vec2 h,t=vec2(.1);
  for(int i=0;i<128;i++){
    h=mp(ro+rd*t.x);
    if(h.x<.0001||t.x>40) break;
    t.x+=h.x;t.y=h.y;
  }
  if(t.x>40) t.y=0;
  return t;
}
#define a(d) clamp(mp(po+no*d).x/d,0.,1.)
#define s(d) smoothstep(0.,1.,mp(po+ld*d).x/d)
void main(void)
{
	vec2 uv = vec2(gl_FragCoord.x / v2Resolution.x, gl_FragCoord.y / v2Resolution.y);
	uv -= 0.5;
	uv /= vec2(v2Resolution.y / v2Resolution.x, 1);
  tt=mod(fGlobalTime,62.82);
  vec3 ro=vec3(cos(tt*c.w+c.x)*c.z,c.y,sin(tt*c.w+c.x)*c.z),
  cw=normalize(vec3(0)-ro),
  cu=normalize(cross(cw,vec3(0,1,0))),
  cv=normalize(cross(cu,cw)),
  rd=mat3(cu,cv,cw)*normalize(vec3(uv,.5)),co,fo,rco;
  co=fo=vec3(.1)-length(uv)*.1;
  ld=normalize(vec3(.1,.3,-.3));
  z=tr(ro,rd);
  t=z.x;
  if(z.y>0){
    po=ro+rd*t;
    no=normalize(e.xyy*mp(po+e.xyy).x+
    e.yxy*mp(po+e.yxy).x+
    e.yyx*mp(po+e.yyx).x+
    e.xxx*mp(po+e.xxx).x);al=vec3(.5);
    float dif=max(0.,dot(no,ld)),
    fr=pow(1+dot(no,rd),4),
    sp=pow(max(dot(reflect(-ld,no),-rd),0),40);
    co=mix(sp+al*(a(.1)+.2)*(dif+s(4)),fo,min(fr,.5));
    co=mix(fo,co,exp(-.0005*t*t*t));
  }

	out_color = vec4(pow(co,vec3(.45)),1);
}
```

Video explaination : https://www.twitch.tv/videos/986628680?t=0h17m38s

## Comment

#define nv(d) d\*mp(po+d).x
no=normalize(nv(e.xyy) + nv(e.yyx) +nv(e.yxy) +nv(e.xxx));
