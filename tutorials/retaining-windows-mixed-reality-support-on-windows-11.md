# Retaining Windows Mixed Reality support on Windows 11

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>Win11 will drop support for WMR devices sometime soon and remove it entirely in Q4 2025.</p></figcaption></figure>

One way of retaining WMR support is to stay on Windows 11 version 23H2 (or below) by disabling Windows feature updates but keeping security updates. _Those are important!_

The easiest way I know for changing what updates you get is by using WinUtil ([https://github.com/ChrisTitusTech/winutil](https://github.com/ChrisTitusTech/winutil)):&#x20;

1. Open Powershell as **admin** and paste: `iwr -useb https://christitus.com/win | iex`
2. Go to the **Updates** tab on top and click **Security Updates.**
3. _You can close the window and/or explore the other features this program provides._

<div align="center" data-full-width="true">

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

</div>

_To revert this change you open WinUtil, go to **Updates** and press **Default Settings.**_
