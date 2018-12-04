document.getElementById("loading-info").style.display = "none";

function prepareMapData(){
  var data = [];
  console.log('in prepareMapData');
  var config = {
    apiKey: "AIzaSyB2VUA6vgbVuzisLKutGOr9sX48Kfg3oxA",
    authDomain: "agk-geo-tracker.firebaseapp.com",
    databaseURL: "https://agk-geo-tracker.firebaseio.com",
    projectId: "agk-geo-tracker",
    storageBucket: "agk-geo-tracker.appspot.com",
    messagingSenderId: "1048812624613"
  };

  firebase.initializeApp(config);
  var db = firebase.firestore();

  var dbRef = db.collection("geo_tracker");
    dbRef.orderBy("time")    
    .get()
    .then(function(querySnapshot) {
      querySnapshot.forEach(function(doc) {
        var temp = {
          latitude : doc.data().latitude,
          longitude : doc.data().longitude
        }
        data.push(temp);
      });
    })
    .catch(function(error) {
        console.log("Error getting documents: ", error);
    });
    return data;
}

  function displayMap() {
    var finalMapData = [];
	  document.getElementById("welcome-div").style.display = "none";
    document.getElementById("loading-info").style.display = "block";

    var data = prepareMapData();
  	
    setTimeout(function(){
      data.forEach(function(res){
      		finalMapData.push(new L.LatLng(res.latitude, res.longitude));
      });

      document.getElementById("loading-info").style.display = "none";

      var map = new L.Map('map');                       
              
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      	attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
  	  }).addTo(map);
      map.attributionControl.setPrefix(''); // Don't show the 'Powered by Leaflet' text.

      var polylineOptions = {
        color: 'red',
        weight: 5,
    	  opacity: 0.9
      };

      var polyline = new L.Polyline(finalMapData, polylineOptions);

      map.addLayer(polyline);                        
      map.fitBounds(polyline.getBounds()); // zoom the map to the polyline
    },5000);          
  }