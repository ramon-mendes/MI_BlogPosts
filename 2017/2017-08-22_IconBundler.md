![](/ContentBlog/iconbudler.png)

IconBundler is a handy desktop tool for **Windows** and **macOSX** to help you work with **icons** in Sciter apps.

With it, you use **[Fontello](http://fontello.com/)** to select which icons you desire, and it will automatically generate the CSS which you `@import` in you HTML for quickly using the icons.

For using it, place the executable in a folder of your project where you want it to create the generated files. Then use the following workflow:

1. run IconBundler app -> Fontello site will automatically open
2. in Fontello, choose the icons you want and then click the 'Save session' button: ![](/ContentBlog/savesession.png) <br> IconBundler automatically regenerates the icon font whenever you 'Save session'
3. in IconBundler, click the icon you want, and in the menu choose the best option to copy the code for using it
4. place the copied HTML in your own HTML where you want the icon to appear

Don't forget to `@import` the generated CSS file. Inside IconBundler you can find the instructions of which `@import` statement to use.

---

**[<i class="icon-download"></i> Download](https://www.dropbox.com/s/8duqumdt1ylf1yo/IconBundlerWin.zip?dl=1)** for Windows (64 bits)

**[<i class="icon-download"></i> Download](https://www.dropbox.com/s/gxdjpt3fggj16v5/IconDropOSX.zip?dl=1)** for macOSX

---

The free version allows you to work with up to 5 icons.

For purchasing full version (**lifetime license**) fill the form bellow:

PURCHASE INDIE: **$69.99**

PURCHASE CORPORATE: **$139.99**

<form method="post" action="/Download/Purchase">
	<input type="hidden" name="redirect" value="/Home/Post/IconBundler" />
	<input type="hidden" name="app" value="IconBuilder" />
	<div class="form-check">
		<label class="form-check-label">
			<input class="form-check-input" type="radio" name="type" value="INDIE" checked />
			Indie
		</label>
		<label class="form-check-label">
			<input class="form-check-input" type="radio" name="type" value="CORPORATE" />
			Corporate
		</label>
	</div>
	<div class="form-group">
		<input type="text" class="form-control" placeholder="Name" name="name" />
	</div>
	<div class="form-group">
		<input type="email" class="form-control" placeholder="E-mail" name="email" />
	</div>
	<div class="form-group">
		<button type="submit" class="btn btn-danger">SUBMIT</button>
	</div>
</form>