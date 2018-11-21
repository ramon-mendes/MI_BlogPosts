In CSS you can declare to attach script behaviors to elements:

```
<style>
	div { behavior: MyBehavior; }
</style>
```

If you want to clear this behavior list definition, you can use ~ :
```
<style>
	div { behavior: ~; }
</style>
```

This is usefull for some tags which have some pre-defined behavior (in engine's master.css stylesheet):
```
<html>
<body>
	<style>
		a { behavior: ~; }
	</style>

	<a href="http://pudim.com.br">This link won't work</a>
</body>
</html>
```
In this example, the <a> anchor element will have its 'hyperlink' behavior removed, so clicking it will not make navigation to happen.