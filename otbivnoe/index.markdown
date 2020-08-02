---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults
---

<!DOCTYPE html>
<html>
<head>
<link href='https://fonts.googleapis.com/css?family=Montserrat' rel='stylesheet'>
<!-- <meta name="viewport" content="width=device-width, initial-scale=1"> -->
<style>
	body {
	  font-family: "Montserrat";
	  background-color: white
	}

	.avatar {
		position: absolute;
		width: 120px;
		height: 120px;
		left: calc(50% - 120px/2);
		top: 80px;
	}

	.name {
		width: 400px;
		font-style: normal;
		font-weight: bold;
		font-size: 20px;
		line-height: 24px;
		text-align: center;
		color: rgba(0, 0, 0, 0.9);
		position: absolute;
		top: 232px;
	}

	.description {
		width: 400px;
		font-style: normal;
		font-weight: 300;
		font-size: 16px;
		line-height: 20px;
		text-align: center;
		color: rgba(0, 0, 0, 0.5);
		opacity: 0.8;
		position: absolute;
		top: 272px;
		white-space: pre-line;
	}

	.background {
		position: absolute;
		width: 400px;
		left: 0px;
		bottom: 0px;
	}

	.stack {
		margin: 0;
		position: absolute;
		top: 50%;
		left: 50%;
		-ms-transform: translate(-50%, -50%);
		transform: translate(-50%, -50%);
		text-align: center;
	}

	.social_container {
		position: absolute;
		height: 24px; 
		font-size:0;
		left: 50%;
		bottom: 40px;
		-ms-transform: translateX(-50%);
		transform: translateX(-50%);
	}

	.social_item {
		display: inline-block; 
		width: 24px;
		height: 24px;
		padding-left: 16px
	}

	.sidenav {
		height: 100%;
		width: 400px;
		position: fixed;
		z-index: 1;
		top: 0;
		left: 0;
		overflow-x: hidden;
		padding-top: 20px;
	}

	.sidenav a {
		padding: 32px 0px 0px 0px;
		text-decoration: none;
	    display: block;
		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 25px;
		line-height: 28px;

		color: #000000;
	}

	.sidenav a:hover {
		color: #E23C2B;
	}

	.sidenav a:hover {
		color: #E23C2B;
	}

	.main {
		margin-left: 400px; /* Same as the width of the sidenav */
		font-size: 28px; /* Increased text to enable scrolling */
		padding: 0px 10px;
	}
</style>
</head>
<body>
<div class="sidenav">
	<img src="/assets/images/side menu/bg.png" class="background">
	<img src="/assets/images/side menu/avatar.png" class="avatar" alt="Avatar">
	<p class="name">Nikita Ermolenko</p>
	<p class="description">iOS Developer &#10;ex-Yandex</p>
	<div class="stack">
		<a href="#contact">Blog</a>
		<a href="#about">About</a>
		<a href="#contact">Contact</a>
	</div>

	<div class="social_container">
		<img src="/assets/images/side menu/twitter.png" class="social_item">
		<img src="/assets/images/side menu/stackoverflow.png" class="social_item">
		<img src="/assets/images/side menu/github.png" class="social_item">
		<img src="/assets/images/side menu/rss.png" class="social_item">
		<img src="/assets/images/side menu/soul.png" class="social_item">
	</div>
</div>

<div class="main">
  <h2>Sidebar</h2>
</div>
   
</body>
</html> 
