# ctx


```
let is={}
is.string = function(obj){return toString.call(obj) === '[object String]'}
is.color=(d)=>{
 if(!is.string(d))return false;
 return /^#?([0-9a-fA-F]{3}|[0-9a-fA-F]{4}|[0-9a-fA-F]{6}|[0-9a-fA-F]{8})$/.test(d)
}
is.transparent=(d)=>{
 if(!is.string(d))return false;
 return /^transparent$/.test(d)
}

////
var pens={};//pens is share
function topen(name,fillflg){
 let p={
 strokeStyle:"transparent"
 ,fillStyle:"transparent"
 ,font:"16px 'Almendra SC',monospace"
 ,textAlign:"left"
 ,textBaseline:"top"
 }
 if(is.color(name))return p[(fillflg)?"fillStyle":"strokeStyle"]=name,p //
 //else
 let ary=pens[name]
 p[(fillflg)?"fillStyle":"strokeStyle"]=ary[0]
 p.font=ary[1]
 return p;
}
//////////////////////


o._img=(src,x0,y0,pen,anim)=>{
 let ctx=o;
 let d=new Image(); d.src=src;
 let w=d.nativeWidth,h=d.nativeHeight
 ctx.drawImage(d,0,0,w,h,x0,y0,w,h)
 ;
 return o.needflip=1,ctx.restore(),o;
}

o._txt=(obj,x0,y0,pen,anim)=>{
 let ctx=o
 Object.assign(ctx,pen) //penset
 ctx.fillText(obj,x0,y0) //native
 ;
 return o.needflip=1,ctx.restore(),o;
}
o._box=(obj,x0,y0,pen,anim)=>{
 let ctx=o
 Object.assign(ctx,pen) //penset
 is.transparent(pen.fillStyle)?ctx.strokeRect(x0,y0,obj[0],obj[1]):ctx.fillRect(x0,y0,obj[0],obj[1])
 //ctx.fillRect(x0,y0,obj[0],obj[1]) //native
 //Object.assign(ctx,fn.clone(o._pen)) //penback
 ;
 return o.needflip=1,ctx.restore(),o;
}
o._poly=(obj,x0,y0,pen,anim)=>{
 let ctx=o
 Object.assign(ctx,pen) //penset
 ctx.beginPath();
 ctx.moveTo(obj[0],obj[1]);
 for(i=2;i<obj.length;i+=2)
  ctx.lineTo(obj[i],obj[i+1]);
 
 ctx.closePath();
 if(!is.transparent(pen.fillStyle)) ctx.fill()
 if(!is.transparent(pen.strokeStyle)) ctx.stroke()
 //Object.assign(ctx,fn.clone(o._pen)) //penback  ?????
 ;
 return o.needflip=1,ctx.restore(),o;
}
;
/*
//dmg("-9",0,0,"#f26",15)//y=(i*i)/2 //(15).toString(16)
o.dmg=(obj,x0,y0,pen,anim)=>{
 let time=anim||15
 let ary=Array.from({length:time}).map((d,i)=>{
  let p={strokeStyle:"#000000",fillStyle:"#f26",font:"24px 'Almendra SC', serif",textAlign:"left",textBaseline:"top"}
  ;
  p.fillStyle=p.fillStyle+(i).toString(16)
  return o.txt.bind(null,obj,x0,y0-((time-i)*(time-i) )/3,p,anim)
 })
 o.anim.push(ary)
 return o;
}
;
*/
o.img=(src,x0,y0,pen,anim)=>{
 ;
 return o.img(src,x0,y0,pen,anim)
}
;
o.txt=(obj,x0,y0,pen,anim)=>{
 return o.txtl(obj,x0,y0,pen,anim)
}
o.txtl=(obj,x0,y0,pen,anim)=>{
 let d=topen(pen,'fill')//fn.clone(o._pen)
 return o._txt(obj,x0,y0,Object.assign(d, {textAlign:"left"}),anim)
}
o.txtr=(obj,x0,y0,pen,anim)=>{
 let d=topen(pen,'fill')//fn.clone(o._pen)
 return o._txt(obj,x0,y0,Object.assign(d, {textAlign:"right"}),anim)
}
o.txtc=(obj,x0,y0,pen,anim)=>{
 let d=topen(pen,'fill')//fn.clone(o._pen)
 return o._txt(obj,x0,y0,Object.assign(d, {textAlign:"center"}),anim)
}

o.full=()=>{
 let w=o._canvas.width,h=o._canvas.height
 return o.box([w,h],0,0,"#000000")
}

o.box=(obj,x0,y0,pen,anim)=>{
 let d=topen(pen,'fill')//fn.clone(o._pen)
 return o._box(obj,x0,y0,d,anim)
}
o.boxb=(obj,x0,y0,pen,anim)=>{
 let d=topen(pen)//fn.clone(o._pen)
 return o._box(obj,x0,y0,d,anim)
}

o.poly=(obj,x0,y0,pen,anim)=>{
 let d=topen(pen,'fill')//fn.clone(o._pen)
 return o._poly(obj,x0,y0,d,anim)
}
o.polyb=(obj,x0,y0,pen,anim)=>{
 let d=topen(pen)//fn.clone(o._pen)
 return o._poly(obj,x0,y0,d,anim)
}


//o.img
//o.txt
//o.txtl
//o.txtr
//o.txtc
//o.box
//o.boxb
//o.full
//o.poly
//o.polyb
//inpd.set(640,480)
//

///////////////////////
function entry(w,h){
 let o={}
 o.pen=(str)=>{
  let a=str.split(',').slice(1),name=a[0];
  pens[name]=a
  return o;
 }
 o.ex=(str)=>{
  str.split('\n').map(d=>{
   let a=str.split(',').slice(1),cmd=a[0];
   return o[cmd].apply(o,a)
  })
  return o;
 }
 o.ef=(str)=>{}
 o.put=(...ary)=>{
  let w=o.canvas.width,h=o.canvas.height
  ary.map(d=>o.putImageData(d.getImageData(0,0,w,h) ,0,0) )
  return o;
 }
 ;
 let canvas=document.createElement('canvas')
 canvas.width=w,canvas.height=h
 let ctx=canvas.getContext('2d')
 return Object.assign(ctx,o)
}
root.ctx=entry;
```

## memo

```
//usage
$a=12
$b=14
$d=444
$image1=im[0]
$layer=ctx.pen('pen1,#000,14 monospace').pen('pen1,#000,14 monospace').pen('pen1,#000,14 monospace')
$layer=$layer(`
txt,aiuewo,20,30,pen1
img,{$image1},20,30,pen2
box,300,20,0,0,#000
`)

```

```
.replace($..>inp.v['$..']

```

```
lay0 marge layer
lay1 base layer
lay2 effect layer
lay3 log layer
lay4 cursor layer
```
```
let lay0=ctx(640,480)
let laybase=ctx(640,480)
let layef=ctx(640,480)
let laylog=ctx(640,480)

lay0.pen('pen1,#000,14 monospace')
lay0.ex('full,#000,0,0,pen1')//paint black
lay0.ex('full,#fff0,0,0,pen1')//paint transparent
lay0.ef('txt,9,#fff,0,0,pen1','color,#f00|pos,3,5',14)
lay0.put(lay1,lay2,lay3)


//effect
lay2.ex('full,#f00,0,0,pen1')
wait>2
lay2.ex('full,#0f0,0,0,pen1')
wait>2
lay2.ex('full,#00f,0,0,pen1')

```
```
let a=ctx(640,480).ex(`
img,${recouse},0,0,#000

`)
a.canvas.id='a',document.body.appendChild(a.canvas)

```



