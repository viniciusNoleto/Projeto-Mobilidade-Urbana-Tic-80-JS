// title:  PROJ Urban Mobolity
// author: viniciusNoleto
// desc:   Projeto de mobilidade com prof. Felipe Loo
// script: js

const constants={
 types:{
  semaphore:0,
		crosswalk:1,
		trafficCircle:2,
	 pedestrian:3,
		bike:4,
	 car:5,
	 motorcycle:6,
		bus:7,
		truk:8,
	}
}

const TURN={
	turnLDplusRU:[33,48,2,17],
	turnLUplusRD:[32,49,1,18],
	turnMIDtoU:[52,21],
	turnMIDtoR:[51,20,117],
	turnMIDtoD:[35,4],
	turnMIDtoL:[36,5,101],
	turnBACKsideU:[24,55],
	turnBACKsideR:[8,39],
	turnBACKsideD:[9,40],
	turnBACKsideL:[25,56],
	trafficRoundabout:[64+16*0,65+16*0,
	                   64+16*1,65+16*1,
	                   64+16*2,65+16*2,
																				64+16*3,65+16*3,
																				64+16*4,65+16*4,
																				64+16*5,65+16*5,
																				64+16*6,65+16*6,
																				70+16*0,71+16*0,
																				70+16*1,71+16*1,
																				70+16*2,71+16*2,
																				70+16*3,71+16*3],
	semaphore:[],
}


var objects=[]


var ObjectModel = function(_sprite,_x,_y,_rotate,
                           _velocity,_type){
 
	var self=this
	
	this.sprite=_sprite
	this.x=_x
	this.y=_y	
	this.rotate=_rotate
	this.velocity=_velocity
	this.type=_type
	
	this.stop=false
	
	this.draw = function(self){
 	spr(self.sprite,self.x-6,self.y,
      0,1,0,0,1,1)
 }
 
 this.commomCurve = function(self,t){
  
  var limit = [[5,1,1,5],[5,5,1,1]]
  var sub = [[3,2,1,0],[1,0,3,2]]
	  
		if((self.y%8==limit[t][0] && self.x%8==limit[t][1])
	 || (self.y%8==limit[t][2] && self.x%8==limit[t][3])){
	  if(self.rotate==0){
	   self.rotate=sub[t][0]
	  }else if(self.rotate==1){
	   self.rotate=sub[t][1]
	  }else if(self.rotate==2){
	   self.rotate=sub[t][2]
	  }else if(self.rotate==3){
	   self.rotate=sub[t][3]
	  }
	 }
		
 }
 
 this.midWayCurve=function(self,t){
  
  var limit = [[1,5,1,1],
               [5,5,1,5],
               [5,1,5,5],
               [1,1,5,1]]
  
  var sub = [[0,3],[1,0],
             [2,1],[3,2]]
     
  if(self.y%8==limit[t][0] 
  && self.x%8==limit[t][1]){
   self.rotate=sub[t][0]
  }
  
  if(self.y%8==limit[t][2] 
  && self.x%8==limit[t][3]){
   self.rotate=sub[t][1]
  }
  
 }
 
 this.goBackCurve = function(self,t){
  
  var limit = [[1,5,1,1],
               [5,5,1,5],
               [5,1,5,5],
               [5,1,1,1]]
  
  var cond = [[0,1,3,0],
              [1,2,0,1],
              [2,3,1,2],
              [3,2,0,3]]
  
  var sub = [[3,2,2,1],
             [0,3,3,2],
             [1,0,0,3],
             [0,1,1,2]]
     
  
  if((self.y%8==limit[t][0]
  &&  self.x%8==limit[t][1])){
   if(self.rotate==cond[t][0]){
    self.rotate=sub[t][0]
   }else if(self.rotate==cond[t][1]){
    self.rotate=sub[t][1]
   }
  }
  
  if((self.y%8==limit[t][2]
  &&  self.x%8==limit[t][3])){
   if(self.rotate==cond[t][2]){
    self.rotate=sub[t][2]
   }else if(self.rotate==cond[t][3]){
    self.rotate=sub[t][3]
   }
  }
  
 }
 
 this.roundStart=0
 this.hasRoundStart=false
 this.countRound=0
 
 this.trafficRoundaboutGo = function(self){
  
  var turn = [[2,false,1,0],[2,3,1,false],
              [2,3,false,0],[false,3,1,0]]
  
  var coord = [[1,1],[5,1],[1,5],[5,5]]
  
  for(var i=0;i<4;i++){
		 if((self.x%8==coord[i][0]
		 &&  self.y%8==coord[i][1])){
		  if(turn[self.roundStart][i]!==false)self.rotate=turn[self.roundStart][i]
		  if(turn[self.roundStart][i]===false)self.countRound++
			}
  }
  
  if(self.countRound==2){
   self.hasRoundStart=false
   self.countRound=0
  }
  
 }
 
 this.curveDetect = function(self){
  
  xX = self.x
  yY = self.y
  
  turnTitles = ["turnLDplusRU","turnLUplusRD",
																"turnMIDtoU","turnMIDtoR",
																"turnMIDtoD","turnMIDtoL",
																"turnBACKsideU","turnBACKsideR",
																"turnBACKsideD","turnBACKsideL",
																"trafficRoundabout"]
  
  
  street=mget(self.x/8,self.y/8)
  
  
  for(var i=0;i<turnTitles.length;i++){
   for(j=0;j<TURN[turnTitles[i]].length;j++){
    if(street==TURN[turnTitles[i]][j]){
     
     if(turnTitles[i] == "turnLDplusRU") this.commomCurve(self,0)
     if(turnTitles[i] == "turnLUplusRD") this.commomCurve(self,1)
     
     if(turnTitles[i] == "turnMIDtoU") this.midWayCurve(self,0)
     if(turnTitles[i] == "turnMIDtoR") this.midWayCurve(self,1)
     if(turnTitles[i] == "turnMIDtoD") this.midWayCurve(self,2)
     if(turnTitles[i] == "turnMIDtoL") this.midWayCurve(self,3)
     
     if(turnTitles[i] == "turnBACKsideU") this.goBackCurve(self,0)
     if(turnTitles[i] == "turnBACKsideR") this.goBackCurve(self,1)
     if(turnTitles[i] == "turnBACKsideD") this.goBackCurve(self,2)
     if(turnTitles[i] == "turnBACKsideL") this.goBackCurve(self,3)
     
     if(turnTitles[i] == "trafficRoundabout"
     && !self.hasRoundStart){
      var coord = [[5,1],[5,5],[1,5],[1,1]]
      
      for(var c=0;c<4;c++){
       if((self.x%8==coord[c][0]
		     &&  self.y%8==coord[c][1])){
								self.roundStart=c
								self.hasRoundStart=true
							}
      }
     }
     
     if(turnTitles[i] == "trafficRoundabout") this.trafficRoundaboutGo(self)
     
    }
   }
  }
 }
 
 direction=[[0,-1*this.velocity],
            [   this.velocity,0],
            [0,   this.velocity],
            [-1*this.velocity,0]]
 
 
 this.move = function(self){
  
  this.curveDetect(self)
  
  self.x+=direction[self.rotate][0]
  self.y+=direction[self.rotate][1]
  
 }
 
 this.att = function(){
  this.draw(this)
  this.move(this)
 }
 
}

var Car = function(_x,_y,_r){
 
 ObjectModel.call(this,259,_x,_y,_r,0.5,4)
 
}

car = new Car(8*8,0*8+5,1)
car2 = new Car(5,8*8,0)
car3 = new Car(21*8,11*8+5,1)
car4 = new Car(25*8+5,8*8,0)


function TIC(){
	
	cls()
 map(0,0,30,17,0,0)
	
	car.att()
	car2.att()
	car3.att()
	car4.att()
	
}
