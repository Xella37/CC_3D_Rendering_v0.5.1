
-- Made by Xelostar: https://www.youtube.com/channel/UCDE2STpSWJrIUyKtiYGeWxw

local function e(p)return math.floor(p+0.5)end
local function t(p,v,b,g,k,q,j)local x={}
for z=1,#p do local _=p[z]local E=
(math.min(
_[1]+v,_[4]+v,_[7]+v)+math.max(_[1]+v,_[4]+v,_[7]+v))*0.5;local T=
(math.min(
_[2]+b,_[5]+b,_[8]+b)+math.max(_[2]+b,_[5]+b,_[8]+b))*0.5;local A=
(math.min(
_[3]+g,_[6]+g,_[9]+g)+math.max(_[3]+g,_[6]+g,_[9]+g))*0.5;local O=k-
E;local I=q-T;local N=j-A
local S=math.pow(O*O+I*I+N*N,0.5)_[14]=S;x[#x+1]=_ end
table.sort(x,function(z,_)return z[14]>_[14]end)return x end
local function a(p,v)local b=fs.open(v.."/"..p,"r")content=b.readAll()
b.close()local g=textutils.unserialise(content)return g end
local function o(p,v,b,g)local p=p+0.0000001;local v=v+0.0000001;local b=b+0.0000001
local k=math.deg(math.atan(p/b))
if(b<0)then if(p<0)then k=k-180 else k=k+180 end else k=k end;local q=k+g;if(q>180)then while(q>180)do q=q-360 end elseif(q<-180)then while(q<-180)do
q=q+360 end end;local j=p/
math.sin(math.rad(k))
local p=j*math.sin(math.rad(q))local b=j*math.cos(math.rad(q))return p,v,b end
local function i(p,v)
if(v~=0)then local b={}
for g,k in pairs(p)do local q,j,x=o(k[1],k[2],k[3],v)
local z,_,E=o(k[4],k[5],k[6],v)local T,A,O=o(k[7],k[8],k[9],v)b[#b+1]={q,j,x,z,_,E,T,A,O}
b[#b][10]=k[10]b[#b][11]=k[11]b[#b][12]=k[12]b[#b][13]=k[13]end;return b else return p end end
local function n(p,v,b,g,k)local q={}
for j=1,#p do local x=p[j]local z=v-x.x;local _=b-x.y;local E=g-x.z
local T=math.pow(z*z+_*_+E*E,0.5)if(T<=k)then x.distance=T;q[#q+1]=x end end
table.sort(q,function(j,x)return j.distance>x.distance end)return q end;local function s(p)local v={x=-p.y,y=p.x}return v end;local function h(p,v)return
(p.x*v.x+p.y*v.y)end
local function r(p,v,b)local g={}g.x=v.x-p.x;g.y=v.y-p.y
local k={}k.x=b.x-v.x;k.y=b.y-v.y
if(h(s(g),k)<0)then return true end;return false end;local function d(p,v,b,g)local k=g-v;local q=b-p;local j=math.pow(10,99)if(q~=0)then j=k/q end
local x=v-j*p;return j,x end
local function l(p,v,b,g,k,q,j,x,z,_)
local E,T=d(b,g,k,q)
if(k>=b)then local A=b>1 and b or 1
for O=A,k do if(O==p)then local I=E*O+T
if(I==v)then return true end end;if(O>z-j+6)then break end end else local A=k>1 and k or 1
for O=A,b do
if(O==p)then local I=E*O+T;if(I==v)then return true end end;if(O>z-j+6)then break end end end
if(q>=g)then local A=g>1 and g or 1
for O=A,q do if(O==v)then local I=(O-T)/E
if(I==p)then return true end end;if(O>_-x+6)then break end end else local A=q>1 and q or 1
for O=A,g do if(O==v)then local I=(O-T)/E
if(I==p)then return true end end;if(O>_-x+6)then break end end end;return false end
local function u(p,v,b,g,k,q,j,x,z,_,E,T)if(j<0)then j=0 elseif(j>T-_+2)then j=T-_+2 end;if(x<0)then x=0 elseif
(x>T-_+2)then x=T-_+2 end
for A=j,x do
if(A==v)then local O=A
if(A~=j and A~=x)then O=e(A)end;local I=e((e(O-0.5)-g)/b)
local N=e((e(O-0.5)-q)/k)if(I<0)then I=0 elseif(I>E-z+2)then I=E-z+2 end;if(N<0)then N=0 elseif
(N>E-z+2)then N=E-z+2 end
if(I<N)then
for S=I,N do if(S==p)then return true end end else for S=N,I do if(S==p)then return true end end end end end;return false end;local function c(p,v,b)return
(p.x-b.x)* (v.y-b.y)- (v.x-b.x)* (p.y-b.y)end
local function m(p,v,b,g,k,q,j,x,z,_,E,T,A)
if(not A)then
local O,I=d(b,g,k,q)local N,S=d(k,q,j,x)local H,R=d(b,g,j,x)
if(g<=q and g<=x)then
if(q<=x)then if(O~=0)then if
(u(p,v,O,I,H,R,g,q,z,_,E,T))then return true end end;if(N~=0)then if
(u(p,v,N,S,H,R,q,x,z,_,E,T))then return true end end else if(H~=0)then if
(u(p,v,O,I,H,R,g,x,z,_,E,T))then return true end end;if(N~=0)then if
(u(p,v,O,I,N,S,x,q,z,_,E,T))then return true end end end elseif(q<=g and q<=x)then
if(g<=x)then if(O~=0)then
if(u(p,v,O,I,N,S,q,g,z,_,E,T))then return true end end;if(H~=0)then if(u(p,v,N,S,H,R,g,x,z,_,E,T))then
return true end end else if(N~=0)then if
(u(p,v,O,I,N,S,q,x,z,_,E,T))then return true end end;if(H~=0)then if
(u(p,v,O,I,H,R,x,g,z,_,E,T))then return true end end end else
if(g<=q)then if(H~=0)then
if(u(p,v,N,S,H,R,x,g,z,_,E,T))then return true end end;if(O~=0)then if(u(p,v,O,I,N,S,g,q,z,_,E,T))then
return true end end else if(N~=0)then if
(u(p,v,N,S,H,R,x,q,z,_,E,T))then return true end end;if(O~=0)then if
(u(p,v,O,I,H,R,q,g,z,_,E,T))then return true end end end end
if(l(p,v,b,g,k,q,z,_,E,T))then return true elseif(l(p,v,k,q,j,x,z,_,E,T))then return true elseif
(l(p,v,j,x,b,g,z,_,E,T))then return true end;return false else
local O=c({x=p,y=v},{x=b,y=g},{x=k,y=q})<0;local I=c({x=p,y=v},{x=k,y=q},{x=j,y=x})<0;local N=
c({x=p,y=v},{x=j,y=x},{x=b,y=g})<0
return((O==I)and(I==N))end end;local f=math.pi;local w=2*math.pi;local y=0.5*math.pi
function newFrame(p,v,b,g,k,q,j,x,z,_,E,T)if(
p==nil or v==nil or b==nil or g==nil)then
error("When creating a new frame, the size must be specified!")end;local A={}A.modelsDir=T or E or
"/models"A.models={}A.modelsSorted={}
A.saveSortedModelsBool=false;A.frameX1=p;A.frameY1=v;A.frameX2=b;A.frameY2=g
A.buffer=bufferAPI.newBuffer(p,v,b,g)A.buffer:clear(colors.white)A.blittleOn=false
A.pixelratio=1.5;A.renderDistance=math.huge;A.FoV=k or 90;A.d=0.2;A.t=
math.tan(math.rad(A.FoV/2))*2*A.d;A.cameraDirZ=0;if(z)then
A.cameraDirZ=math.rad(z)end;A.cameraDirY=0
if(_)then A.cameraDirY=math.rad(_)end;A.cameraX=q or 0;A.cameraY=j or 0;A.cameraZ=x or 0
A.loadedBackgroundColor=false
function A:setSize(p,v,b,g)self.frameX1=p;self.frameY1=v;self.frameX2=b;self.frameY2=g
if(not
self.blittleOn)then self.buffer:setBufferSize(p,v,b,g)self.width=
b-p+1;self.height=g-v+1;self.pixelratio=1.5 else self.buffer:setBufferSize(
p*2,v*3,b*2,g*3)
self.width=(b-p+1)*2;self.height=(g-v+1)*3;self.pixelratio=1 end end
function A:useBLittle(O)self.blittleOn=O
if(O)then
self.buffer:setBufferSize(
(self.frameX1-1)*2+1,(self.frameY1-1)*3+1,(self.frameX2)*2,(
self.frameY2+1)*3)
self.width=(self.frameX2-self.frameX1+1)*2
self.height=(self.frameY2-self.frameY1+1)*3;self.pixelratio=1 else
self.buffer:setBufferSize(self.frameX1,self.frameY1,self.frameX2,self.frameY2)self.width=self.frameX2-self.frameX1+1;self.height=
self.frameY2-self.frameY1+1
self.pixelratio=1.5 end end
function A:setCamera(q,j,x,_,z,k)self.cameraX=q+0.001;self.cameraY=j+0.001
self.cameraZ=x+0.001
if(_~=nil)then self.cameraDirY=math.rad(_)+0.001 end
if(z~=nil)then self.cameraDirZ=math.rad(z)+0.001 end;if(k~=nil)then self.FoV=k end end;function A:setRenderDistance(O)self.renderDistance=O end
function A:loadModel(O,I)local N={}
for S,H in
pairs(I)do N[#N+1]={}N[#N][1]=H.x1;N[#N][2]=H.y1;N[#N][3]=H.z1
N[#N][4]=H.x2;N[#N][5]=H.y2;N[#N][6]=H.z2;N[#N][7]=H.x3;N[#N][8]=H.y3
N[#N][9]=H.z3;N[#N][10]=H.forceRender;N[#N][11]=H.c;N[#N][12]=H.char
N[#N][13]=H.charc end;self.models[O]=N end
function A:getModel(O)local I=self.models[O]local N={}
for S,H in pairs(I)do
N[S]={x1=H[1],y1=H[2],z1=H[3],x2=H[4],y2=H[5],z2=H[6],x3=H[7],y3=H[8],z3=H[9],forceRender=H[10],c=H[11],char=H[12],charc=H[13]}end;return N end;function A:saveSortedModels(O)self.saveSortedModelsBool=O end
function A:findPointOnScreen(O,I,N)local S=
O-self.cameraX;local H=I-self.cameraY
local R=N-self.cameraZ;local D=math.atan(S/R)
if(R<0)then if(S<0)then D=D-f else D=D+f end end;local L=D+self.cameraDirY
if(L>f)then L=L-w elseif(L<-f)then L=L+w end;local U=S/math.sin(D)local S=U*math.sin(L)
local R=U*math.cos(L)local C=math.atan(H/S)
if(S<0)then if(H<0)then C=C-f else C=C+f end end;local M=C-self.cameraDirZ
if(M>f)then M=M-w elseif(L<-f)then M=M+w end;if(M>=y or M<=-y)then return 0,0,false end
local F=S/math.cos(C)local H=F*math.sin(M)local S=F*math.cos(M)
local W=math.atan(S/R)if(R<0)then if(S<0)then W=W-f else W=W+f end end;local Y=0
if
(W>0 and W<y)then Y=self.d/math.tan(W)elseif(W>=y and W<f)then Y=self.d*-
math.tan(W-y)end;if(W>=f or W<=0)then return 0,0,false end
local P=
Y/self.t*self.width+math.floor(self.width*0.5)+1
local V=-self.d*math.tan(M)*self.width/ (self.t*self.height*
self.pixelratio)*
self.height+
math.floor(self.height*0.5)
if(V~=V)then V=math.floor(self.height*0.5)end;return P,V,true end
function A:loadObject(O)local I=O.x;local N=O.y;local S=O.z
local H,R,D=self:findPointOnScreen(I,N,S)
if(H~=nil)then local L=O.model
if(L=="flip")then local U=O.width;local C=O.height;local p,v=self:findPointOnScreen(I,
N-0.5+C,S)
local b,g=self:findPointOnScreen(I,N-0.5,S)local M=math.abs(g-v)local F=1;if(self.blittleOn)then
F=math.abs(M*U/C)else F=math.abs(M*U/C*1.5)end
M=math.floor(M)F=math.floor(F)local W=O.color
if(p~=nil and v~=nil and b~=nil and
g~=nil)then self.buffer:loadBox(p- (0.5*F),v,b+
(0.5*F),g,W,W," ")end else local U={}if(self.models[L]==nil)then
self:loadModel(L,a(L,self.modelsDir))end;U=self.models[L]
local C=O.rotationY~=nil and
O.rotationY~=0 and i(U,O.rotationY)or U
local M=t(C,I,N,S,self.cameraX,self.cameraY,self.cameraZ)
if(self.saveSortedModelsBool)then
if
(O.rotationY==nil or O.rotationY==0)then if(self.modelsSorted[L]==nil)then self.models[L]=M
self.modelsSorted[L]=true end end end
for F=1,#M do local W=M[F]
local p,v,Y=self:findPointOnScreen(I+W[1],N+W[2],S+W[3])
if(Y)then
local b,g,P=self:findPointOnScreen(I+W[4],N+W[5],S+W[6])
if(P)then
local V,B,G=self:findPointOnScreen(I+W[7],N+W[8],S+W[9])
if(G)then if
(W[10]or r({x=p,y=v},{x=b,y=g},{x=V,y=B}))then
self.buffer:loadTriangle(e(p),e(v),e(b),e(g),e(V),e(B),W[11],W[12],W[13])end end end end end end end end
function A:loadObjects(O)if(not self.loadedBackgroundColor)then
self.buffer:clear(colors.white)end
local I=n(O,self.cameraX,self.cameraY,self.cameraZ,self.renderDistance)self.modelsSorted={}
for N=1,#I do self:loadObject(I[N])end;self.loadedBackgroundColor=false;return I end
function A:drawBuffer()self.buffer:drawBuffer(self.blittleOn)end
function A:loadGround(O,I,N)local S=10
local H=S*math.cos(self.cameraDirY)+self.cameraX
local R=S*math.sin(self.cameraDirY)+self.cameraZ
local D,L=self:findPointOnScreen(H,self.cameraY-0.01,R)if(L<0)then L=0 end;if(L>
(self.frameY2-self.frameY1+2)*3)then L=
(self.frameY2-self.frameY1+2)*3 end
if(not
self.blittleOn)then
self.buffer:loadBox(1,L,self.frameX2-self.frameX1+1,
self.frameY2-self.frameY1+2,N or O,O,I or" ")else
self.buffer:loadBox(1,L,(self.frameX2-self.frameX1+1)*
2,(self.frameY2-self.frameY1+2)*3,
N or O,O,I or" ")end;self.loadedBackgroundColor=true end
function A:loadSky(O,I,N)local S=10
local H=S*math.cos(self.cameraDirY)+self.cameraX
local R=S*math.sin(self.cameraDirY)+self.cameraZ
local D,L=self:findPointOnScreen(H,self.cameraY-0.01,R)if(L<0)then L=0 end;if(L>
(self.frameY2-self.frameY1+2)*3)then L=
(self.frameY2-self.frameY1+2)*3 end
if(not
self.blittleOn)then
self.buffer:loadBox(1,1,self.frameX2-self.frameX1+1,L,
N or O,O,I or" ")else
self.buffer:loadBox(1,1,(self.frameX2-self.frameX1+1)*
2,L,N or O,O,I or" ")end;self.loadedBackgroundColor=true end
function A:getObjectIndexTrace(O,I,N,S)local H={}if(self.blittleOn)then I=I*2;N=N*3 end
for W=1,#O do local Y=O[W]
local P={}
if(self.models[Y.model]~=nil)then
P=self.models[Y.model]else P=a(Y.model,self.modelsDir)self.models[Y.model]=P end;local V=
Y.rotationY~=nil and Y.rotationY~=0 and i(P,Y.rotationY)or P
for B=1,#V do
local G=V[B]
local p,v,K=self:findPointOnScreen(Y.x+G[1],Y.y+G[2],Y.z+G[3])
local b,g,Q=self:findPointOnScreen(Y.x+G[4],Y.y+G[5],Y.z+G[6])
local J,X,Z=self:findPointOnScreen(Y.x+G[7],Y.y+G[8],Y.z+G[9])
if
(G[10]or r({x=p,y=v},{x=b,y=g},{x=J,y=X}))then
if(K and Q and Z)then
if(not self.blittleOn)then
if
(m(I,N,e(p),e(v),e(b),e(g),e(J),e(X),self.frameX1,self.frameY1,self.frameX2,self.frameY2,S))then H[#H+1]={objectIndex=W,polygonIndex=B}end else
if
(m(I,N,e(p),e(v),e(b),e(g),e(J),e(X),(self.frameX1-1)*2+1,
(self.frameY1-1)*3+1,(self.frameX2)*2,(self.frameY2+1)*3,S))then H[#H+1]={objectIndex=W,polygonIndex=B}end end end end end end;if(#H<=0)then return end;local R={}local D=-1;local L=math.huge
for W=1,#H do
local Y=O[H[W].objectIndex]local P=self.cameraX-Y.x;local V=self.cameraY-Y.y
local B=self.cameraZ-Y.z;local G=math.pow(P*P+V*V+B*B,0.5)if(G<L)then L=G
D=H[W].objectIndex end end
for W=1,#H do local Y=O[H[W].objectIndex]local P=self.cameraX-Y.x;local V=self.cameraY-
Y.y;local B=self.cameraZ-Y.z
local G=math.pow(P*P+V*V+B*B,0.5)if(G==L)then R[#R+1]=H[W].polygonIndex end end;local U=O[D]local C={}
if(self.models[U.model]~=nil)then
C=self.models[U.model]else C=a(U.model,self.modelsDir)self.models[U.model]=C end;local M=-1;local F=math.huge
for W=1,#R do local Y=C[R[W]]
local P=
(
math.min(unpack({Y[1]+U.x,Y[4]+U.x,Y[7]+U.x}))+
math.max(unpack({Y[1]+U.x,Y[4]+U.x,Y[7]+U.x})))/2
local V=
(
math.min(unpack({Y[2]+U.y,Y[5]+U.y,Y[8]+U.y}))+
math.max(unpack({Y[2]+U.y,Y[5]+U.y,Y[8]+U.y})))/2
local B=
(
math.min(unpack({Y[3]+U.z,Y[6]+U.z,Y[9]+U.z}))+
math.max(unpack({Y[3]+U.z,Y[6]+U.z,Y[9]+U.z})))/2;local G=self.cameraX-P;local K=self.cameraY-V
local Q=self.cameraZ-B;local J=math.pow(G*G+K*K+Q*Q,0.5)
if(J<F)then F=J;M=R[W]end end;return D,M end;return A end