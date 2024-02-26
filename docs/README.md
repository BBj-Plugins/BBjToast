# BBjToast Widget

<p>
  <a href="http://www.basis.cloud/downloads">
    <img src="https://img.shields.io/badge/BBj-v24.00-blue" alt="BBj v24.00" />
  </a>
  <a href="https://github.com/BBj-Plugins/BBjToast/blob/master/README.md">
    <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="BBjToast is released under the MIT license." />
  </a>
  <a href="https://github.com/necolas/issue-guidelines/blob/master/CONTRIBUTING.md#pull-requests">
    <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg" alt="PRs welcome!" />
  </a>
   <a href="https://basishub.github.io/basis-next/#/dwc/dwc-toast">
    <img src="https://img.shields.io/badge/Component-dwc--toast-%23006aff" alt="Tag Name">
  </a>
</p>

BBjToast is a custom BBj Widget for the DWC which allows you to add notifications to your app with ease.

Toast notifications are commonly used in modern applications.
They can be used to provide feedback about an operation or to display a system message.

## Features

- Easy to set up
- Easy to customize
- Support HTML messages
- Has `ON_OPENED` and `ON_CLOSED` callbacks with ability to add custom callbacks for custom button.
- Support Custom Buttons
- Works with the DWC Dark mode 🌒

And much more !

## Installation

* Clone the [project](https://github.com/BBj-Plugins/BBjToast) locally , then add BBjToast to your BBj paths
* Or [Use the plugins manager](https://www.bbj-plugins.com/en/get-started)

## The gist

```BBj
use ::BBjToast/BBjToast.bbj::BBjToast

wnd! = BBjAPI().openSysGui("X0").addWindow(10,10,120,50,"BBjToast")
wnd!.setCallback(BBjAPI.ON_CLOSE,"eoj")

open! = wnd!.addButton(205,10,10,100,30,"Show Toast")
open!.setCallback(open!.ON_BUTTON_PUSH,"open")

toast! = new BBjToast(wnd!)

process_events

open:
  toast!.open("A nice BBjToast widget.")
return

eoj:
release
```


## Placements

A toast can be placed on the screen in six different places

Supported placements are:

- PLACEMENT_TOP_LEFT
- PLACEMENT_TOP
- PLACEMENT_TOP_RIGHT
- PLACEMENT_BOTTOM_LEFT
- PLACEMENT_BOTTOM
- PLACEMENT_BOTTOM_RIGHT
- PLACEMENT_CENTER

```html preview
<div class="placement-sample">
  <dwc-toast message="A nice BBjToast widget."></dwc-toast>
  <div>
    <dwc-button data-placement="top-left">Top Left</dwc-button>
    <dwc-button data-placement="top">Top</dwc-button>
    <dwc-button data-placement="top-right">Top Right</dwc-button>
  </div>
  <br />
  <div>
    <dwc-button data-placement="bottom-left">Bottom Left</dwc-button>
    <dwc-button data-placement="bottom">Bottom</dwc-button>
    <dwc-button data-placement="bottom-right">Bottom Right</dwc-button>
  </div>
</div>
<style>
  dwc-button {
    width: 120px;
    height: 30px;
  }
</style>

<script>
  const toast = document.querySelector(".placement-sample dwc-toast");
  [...document.querySelectorAll(".placement-sample dwc-button")].forEach(
    (x) => {
      x.addEventListener("click", (ev) => {
        const control = ev.target;
        toast.placement = control.dataset.placement;
        toast.open();
      });
    }
  );
</script>
```

```BBj preview
use ::BBjToast/BBjToast.bbj::BBjToast

wnd! = BBjAPI().openSysGui("X0").addWindow(10,10,430,100,"BBjToast")
wnd!.setCallback(BBjAPI.ON_CLOSE,"eoj")

toast! = new BBjToast(wnd!)
id = BBjAPI().getSysGui().getAvailableContext()
_x = 10, x = _x, y = 10, w = 130, h=30

for i=1 to 2
  for j=1 to 3
    dread label$
    dread position
    button! = wnd!.addButton(id, x, y, w, h, label$, $$)
    button!.setUserData(str(position))
    button!.setCallback(button!.ON_BUTTON_PUSH,"onClick")
    id = id + 1
    x = x + w + 10
  next j
  x = _x
  y = y + h + 10
next i

process_events

onClick:
  ev! = BBjAPI().getLastEvent()
  button! = ev!.getControl()
  toast!.setPlacement(num(button!.getUserData()))
  toast!.open("A nice BBjToast widget.")
return

eoj:
release

rem Label , position pairs
data "Top Left"
data BBjToast.PLACEMENT_TOP_LEFT

data "Top"
data BBjToast.PLACEMENT_TOP

data "Top Right"
data BBjToast.PLACEMENT_TOP_RIGHT

data "Bottom Left"
data BBjToast.PLACEMENT_BOTTOM_LEFT

data "Bottom"
data BBjToast.PLACEMENT_BOTTOM

data "Bottom Right"
data BBjToast.PLACEMENT_BOTTOM_RIGHT
```

## Duration

Durations is the time in milliseconds before the toast closes itself.

You can control the toast's duration be calling the `BBjToast::setDuration` method
or by passing the duration directly in the `BBjToast::open` method.

To prevent the toast from closing automatically use a negative number.

?> **Tip** By default , the toast will not be closed automatically as long as the user is hovering over it.

```html preview
<div class="duration-sample">
  <dwc-toast
    message="The toast will close itself after 5s."
    duration="5000"
  ></dwc-toast>
  <dwc-button>Open</dwc-button>
</div>

<script>
  const toast = document.querySelector(".duration-sample dwc-toast");
  const button = document.querySelector(".duration-sample dwc-button");
  button.addEventListener("click", (ev) => {
    toast.open();
  });
</script>
```

```BBj preview
use ::BBjToast/BBjToast.bbj::BBjToast

wnd! = BBjAPI().openSysGui("X0").addWindow(10,10,130,50,"BBjToast")
wnd!.setCallback(BBjAPI.ON_CLOSE,"eoj")

open! = wnd!.addButton(205,10,10,110,30,"open")
open!.setCallback(open!.ON_BUTTON_PUSH,"open")

process_events

open:
  ev! = BBjAPI().getLastEvent()
  button! = ev!.getControl()
  toast! = new BBjToast(wnd!)
  toast!.open("The toast will close itself after 5s.", 5000)
return

eoj:
release
```

## Events

You can listen to the toast's `opened` and `closed` events by setting a callback using the `BBjToast:setCallback` method

```html preview
<div class="events-sample">
  <dwc-toast message="A nice BBjToast widget."></dwc-toast>
  <dwc-button>Open</dwc-button>
</div>

<script>
  const toast = document.querySelector(".events-sample dwc-toast");
  toast.addEventListener("dwc-opened", (ev) => alert("The toast is opened"));
  toast.addEventListener("dwc-closed", (ev) => alert("The toast is closed"));

  const button = document.querySelector(".events-sample dwc-button");
  button.addEventListener("click", (ev) => {
    toast.open();
  });
</script>
```

```BBj preview
use ::BBjToast/BBjToast.bbj::BBjToast

wnd! = BBjAPI().openSysGui("X0").addWindow(10,10,130,50,"BBjToast")
wnd!.setCallback(BBjAPI.ON_CLOSE,"eoj")

open! = wnd!.addButton(205,10,10,110,30,"open")
open!.setCallback(open!.ON_BUTTON_PUSH,"open")

toast! = new BBjToast(wnd!)
toast!.setCallback(BBjToast.ON_OPENED,"onOpen")
toast!.setCallback(BBjToast.ON_CLOSED,"onClose")

process_events

open:
  ev! = BBjAPI().getLastEvent()
  button! = ev!.getControl()
  toast!.open("A nice BBjToast widget.")
return

onOpen:
  let x=msgbox("The toast is opened", 0, "Opened Event")
return

onClose:
  let x=msgbox("The toast is closed", 0, "Closed Event")
return

eoj:
release
```

## Buttons

A toast can have one or more buttons to react to the user actions. You can add a BBjToast's button by adding it to the `BBjToast::Buttons` HashMap.

Every button has two properties. The name of the button `HashMap key` and the custom action of the button `HashMap value`. When the user clicks on the button then the BBjToast will invoke a custom callback using the action name assigned to the button.

```html preview
<div class="buttons-sample">
  <dwc-toast message="The application has new update available" duration="-1">
    <button data-action="close">Later</button>
    <button data-action="update">Update Now</button>
  </dwc-toast>
  <dwc-button>Update</dwc-button>
</div>

<script>
  const toast = document.querySelector(".buttons-sample dwc-toast");

  const update = document.querySelector('[data-action="update"');
  update.addEventListener("click", (ev) => {
    toast.close();
    alert("The application update process will start");
  });

  const open = document.querySelector(".buttons-sample dwc-button");
  open.addEventListener("click", (ev) => {
    toast.open();
  });
</script>
```

```BBj preview
use ::BBjToast/BBjToast.bbj::BBjToast

wnd! = BBjAPI().openSysGui("X0").addWindow(10,10,130,50,"BBjToast")
wnd!.setCallback(BBjAPI.ON_CLOSE,"eoj")

open! = wnd!.addButton(205,10,10,110,30,"Update")
open!.setCallback(open!.ON_BUTTON_PUSH,"open")

toast! = new BBjToast(wnd!)
toast!.getButtons().put("Later", BBjToast.ACTION_CLOSE)
toast!.getButtons().put("Update Now", "update")
toast!.setCallback("update", "onUpdate")

process_events

open:
  ev! = BBjAPI().getLastEvent()
  button! = ev!.getControl()
  toast!.open("The application has new update available", -1)
return

onUpdate:
  toast!.close()
  let x=msgbox("The application update process will start", 0, "Software Update")

return

eoj:
release
```

In case you only want to have a close button then you call the `BBjToast::setClosable` method. This will atomically add a close button for
toast.

```html preview
<div class="closeable-sample">
  <dwc-toast message="This a toast with a close button" duration="-1">
    <button data-action="close">
      <dwc-icon name="x"></dwc-icon>
    </button>
  </dwc-toast>
  <dwc-button style="width: 160px">Closeable Toast</dwc-button>
</div>

<script>
  const toast = document.querySelector(".closeable-sample dwc-toast");
  const open = document.querySelector(".closeable-sample dwc-button");
  open.addEventListener("click", (ev) => {
    toast.open();
  });
</script>
```

```BBj preview
use ::BBjToast/BBjToast.bbj::BBjToast

wnd! = BBjAPI().openSysGui("X0").addWindow(10,10,130,50,"BBjToast")
wnd!.setCallback(BBjAPI.ON_CLOSE,"eoj")

open! = wnd!.addButton(205,10,10,110,30,"Closeable Toast")
open!.setCallback(open!.ON_BUTTON_PUSH,"open")

toast! = new BBjToast(wnd!)
toast!.setCloseable(1)

process_events

open:
  ev! = BBjAPI().getLastEvent()
  button! = ev!.getControl()
  toast!.open("This a toast with a close button", -1)
return

eoj:
release
```

## Themes

Like other BBj Controls, The BBjToast supports themes. You can set the toast's theme using the `BBjToast:setTheme` method.

Supported themes are :

- BBjToast.THEME_DEFAULT
- BBjToast.THEME_PRIMARY
- BBjToast.THEME_SUCCESS
- BBjToast.THEME_WARNING
- BBjToast.THEME_DANGER
- BBjToast.THEME_INFO

```html preview
<div class="theme-sample">
  <dwc-toast message="A nice BBjToast widget."></dwc-toast>
  <div>
    <dwc-button data-theme="default">Default</dwc-button>
    <dwc-button data-theme="primary">Primary</dwc-button>
    <dwc-button data-theme="success">Success</dwc-button>
  </div>
  <br />
  <div>
    <dwc-button data-theme="warning">Warning</dwc-button>
    <dwc-button data-theme="danger">Danger</dwc-button>
    <dwc-button data-theme="info">Info</dwc-button>
  </div>
</div>
<style>
  dwc-button {
    width: 120px;
    height: 30px;
  }
</style>

<script>
  const toast = document.querySelector(".theme-sample dwc-toast");
  [...document.querySelectorAll(".theme-sample dwc-button")].forEach((x) => {
    x.addEventListener("click", (ev) => {
      const control = ev.target;
      toast.theme = control.dataset.theme;
      toast.open();
    });
  });
</script>
```

```BBj preview
use ::BBjToast/BBjToast.bbj::BBjToast

wnd! = BBjAPI().openSysGui("X0").addWindow(10,10,430,100,"BBjToast")
wnd!.setCallback(BBjAPI.ON_CLOSE,"eoj")

toast! = new BBjToast(wnd!)
id = BBjAPI().getSysGui().getAvailableContext()
_x = 10, x = _x, y = 10, w = 130, h=30

for i=1 to 2
  for j=1 to 3
    dread label$
    dread theme
    button! = wnd!.addButton(id, x, y, w, h, label$, $$)
    button!.setUserData(str(theme))
    button!.setCallback(button!.ON_BUTTON_PUSH,"onClick")
    id = id + 1
    x = x + w + 10
  next j
  x = _x
  y = y + h + 10
next i

process_events

onClick:
  ev! = BBjAPI().getLastEvent()
  button! = ev!.getControl()
  toast!.setTheme(num(button!.getUserData()))
  toast!.open("A nice BBjToast widget.")
return

eoj:
release

rem Theme , theme-constant pairs
data "Default"
data BBjToast.THEME_DEFAULT

data "Primary"
data BBjToast.THEME_PRIMARY

data "Success"
data BBjToast.THEME_SUCCESS

data "Warning"
data BBjToast.THEME_WARNING

data "Danger"
data BBjToast.THEME_DANGER

data "info"
data BBjToast.THEME_INFO
```

It is also really easy to create new custom themes

```html preview
<style>
  .toast-custom-theme {
    --dwc-toast-background: linear-gradient(
      to right,
      hsl(203, 100%, 48%),
      hsl(1, 89%, 51%)
    );
    --dwc-toast-color: white;
    --dwc-toast-button-color: white;
    --dwc-toast-border-color: hsl(0, 0%, 73%);
  }

  dwc-button {
    width: 140px;
  }
</style>

<div class="custom-theme-sample">
  <dwc-toast message="The application has new update available" duration="-1" class="toast-custom-theme">
    <button data-action="close">Later</button>
    <button data-action="close">Update Now</button>
  </dwc-toast>
  <dwc-button>Custom Theme</dwc-button>
</div>

<script>
  const toast = document.querySelector(".custom-theme-sample dwc-toast");
  const open = document.querySelector(".custom-theme-sample dwc-button");
  open.addEventListener("click", (ev) => {
    toast.open();
  });
</script>
```

```BBj preview
use ::BBjToast/BBjToast.bbj::BBjToast

style! = "
: .toast-custom-theme { 
:  --dwc-toast-background: linear-gradient(to right, hsl(203, 100%, 48%), hsl(1, 89%, 51%));
:  --dwc-toast-color: white;
:  --dwc-toast-button-color: white;
:  --dwc-toast-border-color: hsl(0, 0%, 73%);
: }
:"

web! = BBjAPI().getWebManager()
web!.injectStyle(style!, 0)

wnd! = BBjAPI().openSysGui("X0").addWindow(10,10,130,50,"BBjToast")
wnd!.setCallback(BBjAPI.ON_CLOSE,"eoj")

open! = wnd!.addButton(205,10,10,110,30,"Update")
open!.setCallback(open!.ON_BUTTON_PUSH,"open")

toast! = new BBjToast(wnd!)
toast!.addClass("toast-custom-theme")
toast!.getButtons().put("Later", BBjToast.ACTION_CLOSE)
toast!.getButtons().put("Update Now", BBjToast.ACTION_CLOSE)

process_events

open:
  ev! = BBjAPI().getLastEvent()
  button! = ev!.getControl()
  toast!.open("The application has new update available", -1)
return

eoj:
release
```
