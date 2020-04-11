# ctx


```
var pens={};//pens is share

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



