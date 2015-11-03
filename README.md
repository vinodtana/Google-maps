
<!DOCTYPE html>
<html>
  <head>
	 <meta name="viewport" content="initial-scale=1.0, user-scalable=no">
	 <meta charset="utf-8">
	 <title>Google map final</title>
	 <style>
		html, body, #map-canvas {
		  height: 100%;
	 width: 1348px;
		  margin: 0;
		  padding: 0; 
		  padding: 0; 
		}

		#panel {
		  position: absolute;
		  top: 0px;
		  left: 13%;
		  margin-left: -180px;
		  z-index: 5;
		  background-color: #fff;
		  padding: 5px;
		  border: 1px solid #999;
		}


		#panel, .panel {
		  font-family: 'Roboto','sans-serif';
		  line-height: 30px;
		  padding-left: 10px;
	 	  width: 350px;
		}

		#panel select, #panel input, .panel select, .panel input {
		  font-size: 15px;
		}

		#panel select, .panel select {
		  width: 100%;
		}

		#panel i, .panel i {
		  font-size: 12px;
		}
		 
	 </style>
  <script src="https://maps.googleapis.com/maps/api/js?v=3.exp&libraries=places&signed_in=true"></script>
  <script>
var map;
var directionsDisplay;
var directionsService;
var stepDisplay;
var markerArray = [];

function initialize() {
  directionsService = new google.maps.DirectionsService();
  var manhattan = new google.maps.LatLng(12.974323, 77.603282);
  var mapOptions = {
	 zoom: 13,
	 center: manhattan
  }
  var map = new google.maps.Map(document.getElementById('map-canvas'), mapOptions);
  var rendererOptions = {
	 map: map
  }
  
  //start----------------------------------
  var input = (document.getElementById('start'));

  var autocomplete1 = new google.maps.places.Autocomplete(input);
  autocomplete1.bindTo('bounds', map);
  
  var infowindow1 = new google.maps.InfoWindow();
  var marker1 = new google.maps.Marker({
	 map: map
  });
  google.maps.event.addListener(marker1, 'click', function() {
	 infowindow1.open(map, marker);
  });

  google.maps.event.addListener(autocomplete1, 'place_changed', function() {
	 infowindow1.close();
	 var place = autocomplete1.getPlace();
	 if (!place.geometry) {
		return;
	 }

	 if (place.geometry.viewport) {
		map.fitBounds(place.geometry.viewport);
	 } 
  else { 
		map.setCenter(place.geometry.location);
		map.setZoom(9);
	 }

	 // Set the position of the marker using the place ID and location
	 marker.setPlace ({
		placeId: place.place_id,
		location: place.geometry.location
	 });
	 marker.setVisible(true);

	 infowindow.setContent('<div><strong>' + place.name + '</strong><br>' +
		  'Place ID: ' + place.place_id + '<br>' +
		  place.formatted_address);
	 infowindow.open(map, marker);
  });
  //end1----------------------------------
  
  var input1 = (document.getElementById('end'));

  var autocomplete = new google.maps.places.Autocomplete(input1);
  autocomplete.bindTo('bounds', map);
  
  var infowindow = new google.maps.InfoWindow();
  var marker = new google.maps.Marker({
	 map: map
  });
  google.maps.event.addListener(marker, 'click', function() {
	 infowindow.open(map, marker);
  });

  google.maps.event.addListener(autocomplete, 'place_changed', function() {
	 infowindow.close();
	 var place = autocomplete.getPlace();
	 if (!place.geometry) {
		return;
	 }

	 if (place.geometry.viewport) {
		map.fitBounds(place.geometry.viewport);
	 } 
  else {
		map.setCenter(place.geometry.location);
		map.setZoom(9);
	 }
  

	 // Set the position of the marker using the place ID and location
	 marker.setPlace ({
		placeId: place.place_id,
		location: place.geometry.location
	 });
	 marker.setVisible(true);

	 infowindow.setContent('<div><strong>' + place.name + '</strong><br>' +
		  'Place ID: ' + place.place_id + '<br>' +
		  place.formatted_address);
	 infowindow.open(map, marker);
  });
  
  //end------------------------------------
  
	 directionsDisplay = new google.maps.DirectionsRenderer(rendererOptions)
	 stepDisplay = new google.maps.InfoWindow();
	 
  <?php 
	 mysql_connect('localhost', 'root', '');
	 mysql_select_db('googlemap1');
	 $sql1 = "SELECT * FROM starting1";
	 $result1 = mysql_query($sql1);
	 
	 while ($row1 = mysql_fetch_array($result1)) {
		?>
		
		var contentString = '<div id="content">'+
		  '<div id="siteNotice">'+
		  '</div>'+
		  '<h1 id="firstHeading" class="firstHeading">Uluru</h1>'+
		  '<div id="bodyContent">'+
		  '<p><b>Uluru</b>, also referred to as <b>Ayers Rock</b>, is a large ' +
		  'Heritage Site.</p>'+
		  '<p>Attribution: Uluru, <a href="https://en.wikipedia.org/w/index.php?title=Uluru&oldid=297882194">'+
		  'https://en.wikipedia.org/w/index.php?title=Uluru</a> '+
		  '(last visited June 22, 2009).</p>'+
		  '</div>'+
		  '</div>';
		var infowindow = new google.maps.InfoWindow({
		  content: contentString,
		  maxWidth: 200
		  });
		  
		  
		var marker = new google.maps.Marker({
		  position: new google.maps.LatLng(<?php echo $row1['Latitude&Longitude']; ?>),
		  map: map,
		  title: '<?php echo $row1['Name'].'\n'.$row1['Address']; ?>'
		  });
		
  <?php }   ?>
	 
	 google.maps.event.addListener(marker, 'click', function() {
		  infowindow.open(map,marker);
		  });
	 
}
function calcRoute() {

	for (var i = 0; i < markerArray.length; i++) {
	 markerArray[i].setMap(null);
  }
  markerArray = [];
  var start = document.getElementById('start').value;
  var end = document.getElementById('end').value;
  //var tm =  document.getElementById('mode').value;
	var request = {
		origin: start,
		destination: end,
		travelMode: document.getElementById('mode').value
	 //travelMode:google.maps.TravelMode."+" tm
  	};
  
  directionsService.route(request, function(response, status) {
	 if (status == google.maps.DirectionsStatus.OK) {
		var warnings = document.getElementById('warnings_panel');
		warnings.innerHTML = '<b>' + response.routes[0].warnings + '</b>';
		directionsDisplay.setDirections(response);
		showSteps(response);
	 }
  });

}
function attachInstructionText(marker, text) {
  google.maps.event.addListener(marker, 'click', function() {
	 stepDisplay.setContent(text);
	 stepDisplay.open(map, marker);
  
  });
	
}

google.maps.event.addDomListener(window, 'load', initialize);

	 </script>
  </head>
  <body>
  <div id="panel">
	 <b>Start: </b><br>
	 <input id="start" type="text" placeholder="Starting Address" style="width: 345px;"/><br>
	 <b>End: </b><br>
	 <input id="end" type="text" placeholder="Ending Address" style="width: 345px;"/><br>
		
	 <b>Mode of Travel: </b>
	 <select id="mode" onchange="calcRoute();">
		  <option> Select Mode Of Travel</option>
		<option value="WALKING">Walking</option>
		<option value="DRIVING">Driving</option>
		<option value="TRANSIT">Transit</option>
	 </select>
	 </div>
	 <div id="map-canvas"></div>

	 <div id="warnings_panel" style="width:100%;height:0%;text-align:center"></div>
  </body>
</html>
