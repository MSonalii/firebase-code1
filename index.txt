(function(){
  document.getElementById("btnLoginDisable").style.display = "none";
  // Initialize Firebase
  var config = {
    apiKey: "AIzaSyC2_m0ebcrCN84PSrhOgNfiD27N1S03fss",
    authDomain: "secondproject-3558b.firebaseapp.com",
    databaseURL: "https://secondproject-3558b.firebaseio.com",
    projectId: "secondproject-3558b",
    storageBucket: "secondproject-3558b.appspot.com",
    messagingSenderId: "651145446950"
  };
  firebase.initializeApp(config);

	var textEmail = document.getElementById('txtEmail');
	var textPassword = document.getElementById('txtPassword');
	var btnLogin = document.getElementById('btnLogin');
	var btnSignup = document.getElementById('btnSignup');
	var btnLogout = document.getElementById('btnLogout');

  if(btnSignup){
    btnSignup.addEventListener('click', function() {
      var email = textEmail.value;
      var password = textPassword.value;
        const promise = firebase.auth().createUserWithEmailAndPassword(email, password);
        promise.catch(function(error) {
          document.getElementById("register-err").innerHTML = error.message;
        });
    });
  }

	if(btnLogin){
		btnLogin.addEventListener('click', function() {
      var email = textEmail.value;
      var password = textPassword.value;
      document.getElementById("btnLoginDisable").style.display = "block";
      document.getElementById("btnLogin").style.display = "none";
  			const promise = firebase.auth().signInWithEmailAndPassword(email, password);
        promise.then(function(res){
          console.log('in success');
          console.log(res);
          window.location.href = 'tab.html';
        });
        promise.catch(function(error) {
          document.getElementById("btnLoginDisable").style.display = "none";
          document.getElementById("btnLogin").style.display = "block";
          document.getElementById("login-err").innerHTML = error.message;
  			});
		});
	}

  /*firebase.auth().onAuthStateChanged(function(user) {
    if (user) {
      document.getElementById("login-err").innerHTML = '';
      window.location.href = 'get-info.html';
      // User is signed in.
    } else {
      // No user is signed in.
    }
  });*/

  if(btnLogout){
    btnLogout.addEventListener('click', function() {
      firebase.auth().signOut();
      window.location.href = 'index.html';
    });
  }
}());