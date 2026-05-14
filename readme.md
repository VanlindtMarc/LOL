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

## Modules ajoutés

### fibo
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

### outline
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

### pythatree
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

### chull

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
### ring
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

### rotate2
```
module rotate2                (){
  rotate([45,90-atan(sqrt(2)),0])
  children();
}
```
