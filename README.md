Image-Slider-using-JavaScript
=============================

Image Slider Using JavaScript


<!DOCTYPE html>

<html>
	<body> 
		<div class="container">
			<div class="image-slider-wrapper">
				<ul id="image_slider">
					<li> <img src="image 1" width ="850px;" height="300px;" /> </li>
                    <li> <img src="image 2" /> </li>
					<li> <img src="image 3" /> </li>
                </ul>
				<div class="pager">
				</div>
			</div>
		</div>
	</body>
</html>

<style type="text/css">
.container{
	width:850px;
	height:300px;
	background: pink;
}
.image-slider-wrapper{
	overflow: hidden;
}
#image_slider{
	position: relative;
	overflow: hidden;
	height: 280px;
}
#image_slider li{
	position:relative;
	 
	float:left;
	  
}
</style>

<script type="text/javascript">
var ul;
var li_items; 
var li_number;
var image_number = 0;
var slider_width = 0;
var image_width;
var current = 0;
function init(){	
	ul = document.getElementById('image_slider');
	li_items = ul.children;
	li_number = li_items.length;
	for (i = 0; i < li_number; i++){
		 	image_width = li_items[i].childNodes[0].clientWidth;
			slider_width += image_width;
			image_number++;
	}
	
	ul.style.width = parseInt(slider_width) + 'px';
	slider(ul);
}

function slider(){		
		animate({
			delay:5,
			duration: 3000,
			delta:function(p){return Math.max(0, -1 + 2 * p)},
			step:function(delta){
					ul.style.left = '-' + parseInt(current * image_width + delta * image_width) + 'px';
				},
			callback:function(){
				current++;
				if(current < li_number-1){
					slider();
				}
				else{
					var left = (li_number - 1) * image_width;					
					setTimeout(function(){goBack(left)},1000); 				
					setTimeout(slider, 3000);
				}
			}
		});
}
function goBack(left_limits){
	current = 0;	
	setInterval(function(){
		if(left_limits >= 0){
			ul.style.left = '-' + parseInt(left_limits) + 'px';
			left_limits -= image_width / 10;
		}	
	}, 5000);
}
function animate(opts){
	var start = new Date;
	var id = setInterval(function(){
		var timePassed = new Date - start;
		var progress = timePassed / opts.duration
		if(progress > 1){
			progress = 1;
		}
		var delta = opts.delta(progress);
		opts.step(delta);
		if (progress == 1){
			clearInterval(id);
			opts.callback();
		}
	}, opts.dalay || 17);
}
window.onload = init;
</script>

