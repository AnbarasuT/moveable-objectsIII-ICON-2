moveable-objectsIII-ICON-2
==========================

activity.js;

var pageContainer, footerContainer,menuContainer, page0, dragContainer, dragActivity, ActivityName, flipDisp=[],random_obj=[],draggable_objects, problem_count = 0,try_again_count = 0, score = 0, Activity_Type, resetBlock = false,savedState,savedState1,savedState2, corAudio, incorTryAudio, incorAudio, playAgain, dots=1;
var bodyTransformedValue = 1;
var navPos=1;
var getState;
var savedValue = false;
var numAudio,interval;
numAudio = new Audio();
numAudio.src = "audio/numerals.mp3";
var scoreSavedState = 0;
var times = 0;
var nextBind = true;
var audioEnd = false;
var audinst=new Audio('audio/instruction.mp3');
var Tryaudio = new Audio("audio/incorrect_try.mp3");
var CorrectAudio = new Audio("audio/number.mp3");
var audio_new = new Audio("audio/correct.mp3");
var Audiovalue = new Audio();
var status = 0;
var numbersAudio = new Array();
for (var num=0; num<20; num++) {
	numbersAudio.push(new Audio("audio/"+((num+1) < 10 ? "0" : "" )+(num+1)+".mp3"));
}
function fullReady(){
	$(".icoInfo").on('click',function () {
	    toggleClassButton($(this),'icoInfoMove');
	    $('.question,#question_blocker').show();
	    audinst.play();
	    //$('.question,#question_blocker').css({'z-index':'1200'})
	})
	$(".icoReset").click(function () {
		  var current=$(this);

		clearInterval(interval);
		try {
			Tryaudio.pause();
			Tryaudio.currentTime = 0;
		} catch(e) {
		}
		DragItem = true;
		navPos=1;
		scoreSavedState = 0;	
		$('#pageBtn2,#pageBtn3,#pageBtn4,#pageBtn5' ).css({'background':'#a0cf65','border': '2px solid #003700'});
		$('#pageBtn1').css({'background':'#003700','border': '#a0cf65'});
	  
	    if (resetBlock) {
		Activity_Type = "reset"
		resetBlock = false;
		$(".btReset").css({'opacity':'0.5','cursor':'default'});
		current.removeClass('icoResetMove');
		dots= 0;
	
		    Activity_Type = "reset"
		 setTimeout(function(){
			current.addClass('icoResetMove');
			},100);
		    
		
			score_box.css({'display':'none'})
			$('.tryagain').css({'display':'block', 'cursor':'pointer','z-index':'2001'})
		
	
			problem_count = 0;
			try_again_count = 0;
			score = 0;
			Activity_Type = "start";
	
		    answer_slot[0].innerHTML = "";
		    reset_function();
		    $(".playAgain").css({'display':'none'});
	
	   }
	});
	
	/*$(document).bind('touchstart',function (){
	for (var num=0; num<20; num++) {
		numbersAudio.push(new Audio("audio/"+((num+1) < 10 ? "0" : "" )+(num+1)+".mp3"));
		numbersAudio[num].play();
		numbersAudio[num].volume = 1;
	}
	});*/
	
 }
$(document).ready(
    function(){
	/* To Inactivate TAB Key */
	
	document.onKeyDown = function(){
		
	    if(window.event && window.event.keyCode == 9) 
            {
		return false; 
	    }
	}
	  
        pageContainer = $("<div id=\"interactive-container\" />");
        menuContainer = $("<div id=\"page-container\" />");
	footerContainer = $("<div class=\"contentHolder\"><div class=\"container\"><div class=\"contentFeedback\"><div class=\"pagebtn\"><div class=\"pagebtn\" id=\"pageBtn1\"></div><div class=\"pagebtn\" id=\"pageBtn2\"></div><div class=\"pagebtn\" id=\"pageBtn3\"></div><div class=\"pagebtn\" id=\"pageBtn4\"></div><div class=\"pagebtn\" id=\"pageBtn5\"></div></div><div class=\"btReset\" ><div class=\"icoReset\"></div></div><div class=\"btInfo\"><div class=\"icoInfo\"></div></div><div class=\"btFeedback\"><div class=\"icoFeed inactive\" id=\"submit\"></div></div><div class=\"score\"><div id=\"contCorrect\"><span id=\"crt\">0</span> <img src=\"images/IconCorrect.png\"></div><div id=\"contIncorrect\"><span id=\"incrt\">0</span><img src=\"images/IconIncorrect.png\"></div></div></div></div></div>");
        page0 = $("<div id=\"page-0\" class=\"page\"></div");
	blocker = $("<div id=\"blocker\" />");
	instruction = $("<div id=\"question_blocker\" onclick=\"closeBtn()\"></div><div class=\"question\"><div style=\"position:absolute; left:2%; top:5%; opacity:0.6;\" id=\"instructions-button\"><img src=\"images/btInfoOn.png\" height=\"32px\" width=\"32px\"  /></div><span style=\"text-align:left; font-size:30px; font-family:GillSansRegular; left:10%; top:15%; line-height:1.4em; width:480px; position:absolute;\">Tap on the blocks to figure out how many there are. Then drag the correct number into the white box.</span><div class=\"closeBtn\" onclick=\"closeBtn()\">&times;</div><div style=\"position:absolute;  left:43%; top:73%;\" onclick=\"AudioIcon()\"><img src=\"images/audioICON.png\" width=\"50px\" height=\"50px\" /></div></div>");
	blocker.css({'position':'absolute','left':'0px','top':'0px','width':'1024px','height':'700px','display':'none'})
        menuContainer.append(page0);
	number_blocker = $("<div id=\"number_blocker\" />");
	number_blocker.css({'position':'absolute','left':'0px','top':'0px','width':'1024px','height':'250px','display':'none'})
        pageContainer.append(menuContainer);
	pageContainer.append(footerContainer);
        
	page0.append(blocker);
	page0.append(instruction);
	numberstrip = $("<div id=\"numberstrip\" />");
	answer_slot_container = $("<div id=\"answer_slot_container\"></div>");
	answer_slot_container.css({'position':'absolute','left':'492px','top':'220px','display':'block'})
	
	answer_slot = $("<div id=\"answer_slot\"></div>");
	page0.append(numberstrip);
	page0.append(answer_slot_container);
	page0.append(answer_slot);
	
	scorePlayagain = $("<div class=\"playAgain\" style=\"display:none;\" onclick=\"playAgain();\">Play Again</div>")
	page0.append(scorePlayagain)
	
	draggable_objects = $("<div id=\"draggable_objects\" />");
	draggable_boundary = $("<div id=\"draggable_boundary\"></div>");
	draggable_boundary.css({'position':'absolute','left':'20px','top':'295px','width':'950px','height':'363px'})
	page0.append(draggable_boundary);
	page0.append(draggable_objects);
	
	feedback = $("<div id=\"feedback\"><div id=\"feedback_text\"></div></div>");
	page0.append(feedback);
	
	try_again = $("<div id=\"try_again\"><span>Try again</span></div>");
	try_again.css({'display':'none'})
	page0.append(try_again);
	
	next_btn = $("<div id=\"next_btn\"><span></span></div>");
	next_btn.css({'display':'none'})
	page0.append(next_btn);
	
	score_box = $("<div id=\"score_box\"></div>");
	score_box.css({'display':'none'})
	page0.append(score_box);
	Activity_Type = "start";
	
$('body').append(pageContainer);
$('body').append('<audio src="audio/numerals.mp3"></audio>');

setTimeout(function (){
	    var matrix = $('body').css('-webkit-transform');
	    if(matrix==null)
	    {
		matrix = $('body').css('transform');
	    }
	    if(matrix!='none' && matrix!=null)
	    {
		var values = matrix.match(/-?[0-9\.]+/g);
		bodyTransformedValue = values[0];
	    }
	    
  $('body').css({'-webkit-transform':'scale(1)'});
fullReady();
saveFunctionality();
random_finder();
reset_function();
}, 1500);

    });

audinst.play();

function saveFunctionality(){

 /*Saved state*/
	eventBroker = _({}).extend(require('chaplin/lib/event_broker'));
eventBroker.publishEvent("#fetch", { type : 'state' }, function(state) {
	if (state) {
		_.each(JSON.parse(state), function(value, key, list) {
			
		    if (key == "resetState") {
			if(value.toString() == "true"){
				resetBlock = true;
				$(".btReset").css({'opacity':'1','cursor':'pointer'});
			}
		    }
		    if (key == "audiostate") {
			savedState2 = value;
			if (savedState2 == 1) {
				audio_status = "on"
				$('.audio-hint').css({'opacity':'0'})
			}
			else{
				audio_status = "off"
				$('.audio-hint').css({'opacity':'0'})
			}
		    }
		if (key == "scores") {
			
			score = value;
			
		}
		if (key == "scoreIncrement") {
			if (value == 1) {
			savedValue = true;	
			}
			if (value == 0) {
			savedValue = false;	
			}
				
		}
		if (key == "tryCount") {
			try_again_count = value;
			  }
		if(key == "button"){
			for (var i=1;i<value;i++) {
				$('#pageBtn'+i).css({'background':'#a0cf65','border': '#003700'});
			}
			$('#pageBtn'+value).css({'background':'#003700','border': '#a0cf65'});
			navPos = value;
			problem_count = value-1;
			audioEnd = false;
			
		    }
		   
                    if(key == "fillDetail") {
                        savedState = value;
			
			$('#answer_slot').html(savedState);
			//console.log('save retrive',$('#answer_slot').html());
		    }
		     if(key == "filldata") {
                        savedState1 = value;
			$('#draggable_objects').html(savedState1);
			dragActivity = "";
		    }
		    
		 
		    });
}
        });

eventBroker.subscribeEvent('#doSave', function(state) {
    var state = {};
    /*Change here for each interactive*/
   console.log(navPos)
    state = {
	"scores":score,
	"scoreIncrement":scoreSavedState,
	"tryCount":try_again_count,
	"fillDetail":$('#answer_slot').html(),
	"filldata":$('#draggable_objects').html(),
	"resetState":resetBlock.toString(),
	"button":navPos,
	
	};
    /**/
    var message = {
	    type : 'state',
	    data : JSON.stringify(state)
    };
    eventBroker.publishEvent("#save", message);
});
}

function random_finder() {
    random_img_array = [];
	
    for(i=0; i<5; i++){
	do{
		//console.log(i, random_img_array)
	    already_exists = false;
	    random_image_1 = Math.floor(Math.random() * 30000)%5;
	    for(j=0; j<random_img_array.length; j++){
		if (random_image_1 == random_img_array[j]) {
		    already_exists = true;
		}
	    }
	}
	while(already_exists == true);
	random_img_array.push(random_image_1)
    }
    for(i=0; i<5; i++){
	already_exists = false;
	for(j=0; j<random_img_array.length; j++){
	    if (i==random_img_array[j]) {
		already_exists = true;
	    }
	}
	if (already_exists == false) {
	    random_img_array.push(i);
	    i=5;
	}
    }
}
function reset_function() {
    $(".icoFeed").unbind('click',icoFeedClick);
    $(".playAgain").unbind('click',playAgain);
    try_again_count = 0
    next_btn.css({'display':'none'})
    try_again.css({'display':'none'})
    $(".icoFeed").removeClass("icoFeedMove")
    if (answer_slot.children().length > 0){
	try{
			$(".icoFeed").unbind('click');
		}
		catch(e){}
		$(".icoFeed").bind('click',icoFeedClick);
		$(".icoFeed").addClass('icoInfoMove');
		$(".icoFeed").addClass('icoFeedMove');
		$(".playAgain").css({'display':'none'})
	}
    $(".btFeedback").css({'cursor':'default'});
    feedback[0].firstChild.innerHTML=""
    $("#blocker").css({'display':'none'})
    flipDisp = [];
    left_pos = 0;
    top_pos = 10;
    top_pos2 = 95;
    numberstrip[0].innerHTML = ""
        try {
	answer_slot.children().eq(0).css({"z-index":"1"});
    } catch(e) { }
    for (i=0; i<=19; i++) {
	if (i <= 9) {
		flipDisp[i] = $("<div id=\"tick" + (i+1) + "-1\" class=\"tick tick-flip\" />");
		flipDisp[i].css({'position':'absolute','left':''+(52+left_pos*94)+'px','top':top_pos+'px'})
		flipDisp[i].html(i+1)
		numberstrip.append(flipDisp[i]);
		left_pos +=1
	}
	else{
		flipDisp[i] = $("<div id=\"tick" + (i+1) + "-1\" class=\"tick tick-flip\" />");
		flipDisp[i].css({'position':'absolute','left':''+(-886+left_pos*94)+'px','top':top_pos2+'px'})
		flipDisp[i].html(i+1)
		numberstrip.append(flipDisp[i]);
		left_pos +=1;
	}
    }
    dragActivity = "";
    
    dragActivity = new DragNDrop(answer_slot,numberstrip);
//    dragActivity.OnAllDropComplete=function(){
//	try{
//	$(".icoFeed").unbind('click');
//	}
//	catch(e){}
//	$(".icoFeed").bind('click',icoFeedClick);
//	savedValue = false;
//	scoreSavedState = 0;
//    }
}
function closeBtn(){
	try {
            audinst.pause();
            audinst.currentTime = 0;
            } catch(e) {}
    $('.question,#question_blocker').hide();
    $(".icoInfo").addClass('icoInfoMove');
}
function icoFeedClick(){
	//try {
	//	numAudio.pause();
	//	numAudio.stop();
	//} catch(e) {}
	scoreSavedState = 1;
	DragItem = true;
	nextBind = true;
    $("#blocker").css({'display':'block'})
    resetBlock = true;
    $('.btReset').css({'opacity':'1','cursor':'default'})
    $('.btFeedback').css({'cursor':'default'})
    var temp = answer_slot.text();
    
    
    if( Number(temp) == $("#draggable_objects").children().length){
	status = 1;
	audio_new.play();
	if (try_again_count==0) {
	if (!savedValue) score+=2;
	    }
	else
	{
	if (!savedValue)  score+=1;
	}
	//next_btn.bind('click');
	next_btn.css({'display':'block'});
	next_btn.bind('click', nextSet);
	audioEnd = false;
	
	//if (navPos < 6) {
	next_activity();
	//}
	feedback.css({'top':'230px','left':'600px','display':'block'})
	feedback[0].firstChild.innerHTML="Correct";
	savedValue = false;
    }
    else{
	savedValue = false;
	status = 2;
	try_again_count+=1;
	if (try_again_count==1) {
            feedback.css({'top':'223px','left':'590px','z-index':'2001','display':'block'})
	    feedback[0].firstChild.innerHTML="Incorrect"
	    try_again.css({'display':'none','z-index':'7001'})
	    DragItem = true;
	    $("#try_again").bind('click',try_againClick);
	}
	else {
		Tryaudio=null;
		Tryaudio = new Audio("audio/incorrect_try.mp3");
		Tryaudio.play();
		
		Tryaudio.addEventListener('loadstart', function (){
			DragItem = false;
			left_val = -65;
			top_val = draggable_boundary.position().top+(draggable_boundary.height()-((random_obj[0].height()*2)+10))/2;
			objWidth = random_obj[0].width()+10;
			objHeight = random_obj[0].height()+10;
			var totalTimes = $('#draggable_objects').children().length;
			var times = 0;
			setTimeout(onLoadStart, 1000);
			function onLoadStart(){
				interval = setInterval(function (){
					if (times==totalTimes-1) {
						setTimeout(function (){
							Tryaudio.pause();
							
							DragItem = false;
					CorrectAudio.play();
					Audiovalue.src = "audio/"+answer_slot.value.toString()+".mp3";
					setTimeout(function(){
						Audiovalue.play();
					},1200);
					console.log(navPos,'navPos');
					if (navPos<5) {
						next_btn.css({'display':'block'});
						next_btn.bind('click', nextSet);
						nextBind = true;
						problem_count++;
						console.log('score',score);
					}else{
						next_btn.css({'display':'none'});
						next_btn.unbind('click');
						nextBind = false;
						problem_count = 4; 
						next_activity();
					}
						
					feedback[0].firstChild.innerHTML="Incorrect<br /><div class=\"solution\">The correct number is "+answer_slot.value+".</div>";
							
							
						}, 1200)
						clearInterval(interval);
						
					}
					if ((times) < 10) {
					left_val += objWidth;
					random_obj[times].animate({"left":left_val+"px","top":top_val+"px"},{complete:function(){},duration:100});
				}
				else if((times) <= totalTimes-1){
					if ((times) == 10){
						left_val = -65;
						top_val += objHeight;
					}
					left_val += objWidth;
					random_obj[times].animate({"left":left_val+"px","top":top_val+"px"},{complete:function(){},duration:100});
				}
				//console.log(times+"=="+totalTimes)
				times++;
				}, 1500);
			}
			
			feedback.css({'top':'223px','left':'590px','z-index':'2001','display':'block'});
			feedback[0].firstChild.innerHTML="Incorrect";	
		});
	
	  // next_activity();
	}
    }
    $(".icoFeed").unbind('click',icoFeedClick);
    toggleClassButton($(".icoFeed"),'icoFeedMove');
}

function toggleClassButton(element,className){
    var currentButton=element;
    if(!currentButton.hasClass(className)){
	currentButton.addClass(className);
    }else{
	currentButton.removeClass(className);	
    }
}
function try_againClick(){
    DragItem = true;
    answer_slot[0].innerHTML = ""
    feedback[0].firstChild.innerHTML=""
    $("#blocker").css({'display':'none'})
    try_again.css({'display':'none'})
    $("#try_again").unbind('click',try_againClick);
}
function next_activity(){
     $(".icoFeed").unbind('click',icoFeedClick);
     $(".icoFeed").removeClass("icoFeedMove");
     console.log("problem_count="+problem_count);
     console.log('score',score);
    problem_count +=1;
    if (problem_count<5) {
	DragItem = false;
	next_btn.css({'display':'block'});
	next_btn.bind('click', nextSet);
	nextBind = true;
    }
   if(problem_count == 5 || problem_count == 6)
    {
	DragItem = false;
	$(".icoFeed").removeClass("icoFeedMove");
	score_box[0].innerHTML = "Score<br />" + score+"/"+(problem_count*2);
	$('.playAgain').css({'display':'block', 'cursor':'pointer','z-index':'5008'})
	//$(".btReset").css({'opacity':'1','cursor':'pointer'});
	next_btn.css({'display':'none'});
	resetBlock = true;
	random_finder();
	if (status == 1) {
		score_box.css({'display':'block','z-index':'5000'});
	}
	if (status == 2) {
			$('.playAgain').css({'display':'block', 'cursor':'pointer','z-index':'5008'})
			score_box.css({'display':'block','z-index':'5000'});
	}
	
	
    } 
}


function nextSet(){

DragItem = true;
if (navPos<5) {
if(nextBind){
navPos++;
nextBind =false;
}
}

//console.log(navPos,'navPos');
$('#pageBtn'+(navPos-1)).css({'background':'#a0cf65','border': '#003700'});
$('#pageBtn'+navPos).css({'background':'#003700','border': '#a0cf65'});

feedback[0].firstChild.innerHTML="";
draggable_objects[0].innerHTML = "";
answer_slot[0].innerHTML = "";
Activity_Type = "start";
next_btn.unbind('click');

reset_function();
}

function saveFunction(){
    eventBroker.publishEvent("#doSave");
}
function playAgain() {
		clearInterval(interval);
		DragItem = true;
		navPos=1;
		scoreSavedState = 0;	
		$('#pageBtn2,#pageBtn3,#pageBtn4,#pageBtn5').css({'background':'#a0cf65','border': '2px solid #003700'});
		$('#pageBtn1').css({'background':'#003700','border': '#a0cf65'});
	  
	   
		Activity_Type = "reset"
		resetBlock = false;
		$(".btReset").css({'opacity':'0.5','cursor':'default'});
		$(".btReset").removeClass('icoResetMove');
		dots= 0;
	
		    Activity_Type = "reset"
		
		
			score_box.css({'display':'none'})
			$('.tryagain').css({'display':'block', 'cursor':'pointer','z-index':'2001'})
		
	
			problem_count = 0;
			try_again_count = 0;
			score = 0;
			Activity_Type = "start";
	
		    answer_slot[0].innerHTML = "";
		    reset_function();
	$("#score_box").css({'display':'none'});
	$(".playAgain").css({'display':'none'});
}
function AudioIcon() {
           if (audinst.paused == false) {
            audinst.pause();
            audinst.currentTime = 0;
             audinst.play();
           }
           else{
            audinst.play();
           }
}
