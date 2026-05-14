L.O.L. est une librairie pour OpenSCAD permettant tout un tas de choses :)

- Nouvelles fonctions
- Nouveaux modules
- Nouvelles formes
- fractales...

Toutes les nouvelles formes peuvent être appelées comme toutes les autres.

Mais toutes les formes peuvent maintenant être appelées en tant que variables.

# Variables ajoutées 

```
LogoFB= [[4.46567, 4.99666], [3.06433, 4.99666], [3.06433, 0], [0.987666, 0], [0.987666, 4.99666], [0, 4.99666], [0, 6.76134], [0.987666, 6.76134], [0.987666, 7.90467], [1.00769, 8.22956], [1.07504, 8.57716], [1.20063, 8.92712], [1.39537, 9.25909], [1.6702, 9.55271], [2.03602, 9.78764], [2.50376, 9.94352], [3.08433, 10], [4.62167, 9.99402], [4.62167, 8.28068], [3.505, 8.28068], [3.35933, 8.26038], [3.21662, 8.18648], [3.10811, 8.0397], [3.065, 7.80073], [3.065, 6.76073], [4.64833, 6.76073]];
 
LetterL=[[0,0],[0,70],[22,73],[19,21],[50,25],[48,-1],[0,0]];
LetterO=[[0,35],[6.25,61.25],[25,70],[25,70],[43.75,61.25],[50,35],[43.75,8.75],[25,0],[6.25,8.75],[0,35]];
//LetterO=[[0,0],[0,70],[50,70],[50,0],[0,0]];
blue			= [0,0,1,1];
red				= [1,0,0,1];
green			= [0,1,0,1];
violet		= [0.5,0,0.5,1];
yellow		= [1,1,0,1];
cyan			= [0,1,1,1];
black			= [0,0,0,1];
white			= [1,1,1,1];
oak				= RVB(200,50,90,255);
orange		= [1,0.5,0,1];
olive			= [0.5,0.5,0,1];
sarcelle	= [0,0.5,0.5,1];
marine		= [0,0,0.5,1];
fuschia		= [1,0,1,1];
glass		  = [1,0,1,0.2];
 
 
bleu			= [0,0,1,1];
rouge			= [1,0,0,1];
vert			= [0,1,0,1];
jaune			= [1,1,0,1];
noir			= [0,0,0,1];
blanc			= [1,1,1,1];
gris			= [0.5,0.5,0.5,1];
gray			= [0.5,0.5,0.5,1];
pink			= RVB(255,107,219,255);

phi       = (1+sqrt(5))/2/1;
aphi      = phi-1;
biphi     = phi+1;
angledor  = 360/biphi;
py        = sqrt(0.5);
bipy      = sqrt(2);
pi        = 3.141592654/1;
tau       = pi*2;
```
# Nouvelles fonctions
## Sur nombres
### random
```
function random    (n,s,pos)       = rands(pos==undef?0:pos==true?0:-n,n,1,s==undef?n:s)[0];
```
### fibonacci
```
function fibonacci (n,a=0,b=1,c=1) = c<n+1?fibonacci(a=b,b=a+b,c=c+1,n=n):a;
```

### hypo
```
function hypo      (a,b)           = sqrt((a*a)+(b*b));
```
### imaginary
```
function imaginary (a,b,c)         = (2*a*b)+c;    
```
### pair
```
function pair      (a)             = a%2==0?true:false;
```

### real
```
function real      (a,b,c)         = ((a*a)+(b*b))+c;
```
## Sur tables
### invert
```
function invert  (a)               = let(b=[for(i=[0:len(a)-1]) a[(len(a)-1)-i]])b;
```

### moyenne
```
function moyenne  (a,b=0,c=0)       = b<len(a)?sum(a=a,b=b+1,c=c+a[b])/len(a):c;
```
### sort
```
function sort    (a,invert=false)  = len(a) == 0 ? [] : let (
      b=floor(len(a)/2),
      c=[for(i=a) if (i<a[b]) i],
      d=[for(i=a) if (i>a[b]) i],
      e=[for(i=a) if (i==a[b]) i]
    ) 
    invert==false?concat(sort(c),e,sort(d)):invert(concat(sort(c),e,sort(d)));
```
### sum
```
function sum     (a,b=0,c=0,n)       = b<(n==undef?len(a):n)?sum(a=a,b=b+1,c=c+a[b],n=n):c;
```
### topct
```
function topct   (a)               = a/sum(a);
```
## Sur vecteurs
### divide
```
function divide         (a,b,c)         = [a[0]+(b[0]-a[0])*c, a[1]+(b[1]-a[1])*c];    
```
### fract
```
function fract(a,angle,in,maxit,it,close)= let( 
	close=close==undef?true:close,
	a=close==true?a[0]==a[len(a)-1]?a:concat(a,[a[0]]):a,
  maxit=maxit==undef?3:maxit==0?1:maxit,
    it=it==undef?0:it,
		inside=in==undef?1:in==true?1:-1,
    angle=angle==undef?60:angle,
  b = [ for ( i = [ 0 : len(a)-2  ] ) [
      a[i],
      divide(a[i],a[i+1],angle/180), 
      divide(a[i],a[i+1],angle/180) + 
			[
		sin(myangle(a[i],a[i+1]) + angle*inside) * (length(a[i],a[i+1])/3) ,	       
		cos(myangle(a[i],a[i+1]) + angle*inside) * (length(a[i],a[i+1])/3)
			], 
      divide(a[i],a[i+1],1-(angle/180)),  
      a[i+1]]
    ]
  )  
  it+1==maxit?clean(join2(b)):fract(a=clean(join2(b)),angle=angle,in=in,maxit=maxit,it=it+1,close=close); 
```

### join
```
function join(a) = [for(i=[0:len(a)-1] ) each a[i]];
```

### koch
```
function koch           (a,angle,maxit,it)       =  let(
a=a[0]==a[len(a)-1]?a:concat(a,[a[0]]),
b = [ for ( i = [ 0 : len(a)-2  ] ) [
      a[i],
      divide(a[i],a[i+1],1/3),
      divide(a[i],a[i+1],1/3) + [
        sin(myangle(a[i],a[i+1])-(angle==undef?60:angle<=60?60:angle>=180?180:angle))*length(a[i], a[i+1])/3,
        cos(myangle(a[i],a[i+1])-(angle==undef?60:angle<=60?60:angle>=180?180:angle))*length(a[i],a[i+1])/3],
 
  divide(a[i],a[i+1],2/3)+[sin(-90+myangle(a[i],a[i+1])-((90+(90-(angle<=60?60:angle>=180?180:angle)))))*length(a[i],a[i+1])/3,cos(-90+myangle(a[i],a[i+1])-(90+(90-(angle<=60?60:angle>=180?180:angle))))*length(a[i],a[i+1])/3],
  divide(a[i],a[i+1],2/3),
  a[i+1]
  ]],
  maxit=maxit==undef?0:maxit,
  it=it==undef?0:it
    ) 
  it==maxit?clean(join2(b)):koch(a=clean(join2(b)),angle=angle,maxit=maxit,it=it+1);
```
### length
```
function length        (a,b)           = sqrt(((b[0]-a[0])*(b[0]-a[0]))+((b[1]-a[1])*(b[1]-a[1])));
```
### myangle
```
function myangle        (a,b)           = atan2(b[0]-a[0],b[1]-a[1]);
```
### normal
```
function normal    (a)             = a/(sqrt(a[0]*a[0]+a[1]*a[1]));
```


---
# Modules ajoutés

## cnc
```
module cnc(d,show,fn){
  show=show==undef?false:show;
  d = d == undef ? 3:d;
  fn = fn == undef ? 32:fn;
  if(show==false){ 
    offset(-d/2,$fn=fn)
    offset(d/2,$fn=fn)
    children();
  }
  else
  {
    color("green")
    linear_extrude(1)
    children();
    color("red")
    linear_extrude(0.5)
    difference()
    {
      offset(-d/2,$fn=fn)
      offset(d/2,$fn=fn)
      children();
      children();
    }
  }
}
```

## chull

```
module chull                  (m){ 
  union()
  for(i=[0:$children-2]){
    hull(){
      children(m==true?0:i);
      children(i+1);
    }
  }
}
```

## fibo
```
module fibo                   (s,n,r){
  r=r==undef?true:r;
  s=s==undef?1:s;
  n=n==undef?128:n;
 
 
  for(i=[1:n]){
    rotate([0,0,angledor*i])
    translate([s*i,0,0])
    scale(r==true?s+pow(1.003,i):1)
    children();
  }  
}
```

## grid
```
module grid                   (dim,x,y){
  dim=dim==undef?[100,100]:dim;
  x=x==undef?10:x;
  y=y==undef?10:y;
  for(i=[1:x-1])
    translate([i*dim[0]/x,0,0])
    rotate([-90,0,0])
    linear_extrude(dim[1])
    children();  
 
  for(i=[1:y-1])
    translate([0,i*dim[1]/y,0])
    rotate([-90,0,-90])
    linear_extrude(dim[0])
    children();  
}
```


## moebius
```
module moebius(d,t,fn){ // version 2.0
  fn = fn == undef ? 128  :fn;
  d = d == undef ? 30   :d;
  t = t == undef ? 0.5 :t;
  union(){
   for(j=[0:$children-1])
   {
    for(i=[1:fn]){
      hull(){
        rotate([0,360/fn*i,0])
        translate([d/2,0,0])
        rotate([0,0,i*(360*t)/fn])
        linear_extrude(0.1)
        children(j);
 
        rotate([0,360/fn*(i+1),0])
        translate([d/2,0,0])
        rotate([0,0,(i+1)*(360*t)/fn])
        linear_extrude(0.1)
        children(j);
      }
    }
  }
 }
}
```


## outline
```
module outline                (w,t){ 
  w=w==undef?1:w;
  t=t==undef?"on":t;
  difference()
  {    
    offset(t=="out"?w:t=="in"?0:w/2)
    children();
    offset(t=="out"?0:t=="in"?-w:-w/2)
    children();
  }
}
```

## pythatree
```
module pythatree              (a,h,sp,maxit,b,r1,r2,s,d,c1,c2,col){ 
  a  = a  == undef ? 45  : a; 
  h  = h  == undef ? 1   : h;
  sp = sp == undef ? 0   : sp;
  maxit  = maxit  == undef ? 3   : maxit;
  b  = b  == undef ? 0   : b;
  r1 = r1 == undef ? 0   : r1;
  r2 = r2 == undef ? 0   : r2;
  s  = s  == undef ? py  : s;
  d  = d  == undef ? "y" : d;
  c1 = c1 == undef ? [0.5,0.25,0] : c1;
  c2 = c2 == undef ? [0.5,1,0] : c2;
  col = [c1[0]+(c2[0]-c1[0])/maxit*(b-1),c1[1]+(c2[1]-c1[1])/maxit*(b-1),c1[2]+(c2[2]-c1[2])/maxit*(b-1)];
  color(col)
  children();
  if(b<=maxit)
  {
    translate([d=="x"?h:d=="y"?sp:-sp, d=="x"?-sp:d=="y"?h:0, d=="x"?0:d=="y"?0:h])
    rotate([d=="x"?0:d=="y"?r2:0, d=="x"?r2:d=="y"?0:-a, d=="x"?-a:d=="y"?-a:r2])
    scale([s,s,s])
    pythatree(a=a,h=h,sp=sp,maxit=maxit,b=b+1,r1=r1,r2=r2,s=s,d=d,c1=c1,c2=c2)
    {
      children();
    };
    translate([d=="x"?h:d=="y"?-sp:sp, d=="x"?sp:d=="y"?h:0, d=="x"?0:d=="y"?0:h])
    rotate([d=="x"?0:d=="y"?r1:0, d=="x"?r1:d=="y"?0:a, d=="x"?a:d=="y"?a:r1])
    scale([s,s,s])
    pythatree(a=a,h=h,sp=sp,maxit=maxit,b=b+1,r1=r1,r2=r2,s=s,d=d,c1=c1,c2=c2)
    {
      children();
    };
  }
}
```


## ring
```
module ring                   (d,n,m){
  d=d==undef?10:d;
  n=n==undef?5:n;
  m=m==undef?0:m;
  for(i=[0:n-1]){
    rotate([0,0,360/n*i]){
      translate([d/2,0,0])
      rotate([0,0,m])
      children();
      echo(m);
    }
  }
}
```

## rotate2
```
module rotate2                (){
  rotate([45,90-atan(sqrt(2)),0])
  children();
}
```
## skew
```
module skew                   (XY,XZ,YX,YZ,ZX,ZY){  
  matrice=[ 
    [1,XY,XZ,0], //[redimX, skewXY, skewXZ,translateX]
    [YX,1,YZ,0], //[SkewYX,RedimY,SkewYZ,translateY]
    [ZX,ZY,1,0] //[SkewZX, SkewZY,redimZ,TranslateZ]
  ]; 
  multmatrix(matrice){
    children();
  }
}
```

# Nouvelles formes 
Les nouvelles formes sont toutes à la fois des fonctions et des modules.
