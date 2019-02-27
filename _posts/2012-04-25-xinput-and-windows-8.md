---
layout: post
title: XINPUT and Windows 8
date: 2012-04-25 18:39
author: walbourn
comments: true
categories: [audio, input, Uncategorized, win8]
---
<p>The <em>Windows 8 Consumer Preview</em>&nbsp;includes version 1.4 of the <a href="http://msdn.microsoft.com/en-us/library/windows/desktop/ee417003.aspx">XInput</a> API for use with Xbox 360 Common Controller compatible game devices, and this API is fully supported for both Windows Store apps (including x86, x64, and Windows on ARM) and desktop Win32 applications. The full details of the differences compared to XInput 1.3 which shipped in the DirectX SDK (June 2010) release are addressed on <a href="http://msdn.microsoft.com/en-us/library/windows/desktop/hh405051.aspx">MSDN</a>. The headers and libraries for Xinput 1.4 are included in the Windows SDK 8.0 that is part <a href="http://blogs.msdn.com/b/chuckw/archive/2012/02/29/visual-studio-11-beta.aspx">Visual Studio 11 Beta</a>. Windows 8 also includes an updated driver for these devices, <code>XUSB22.SYS</code>.</p>
<p><strong>Update: </strong>This information also applies to <a href="http://blogs.msdn.com/b/chuckw/archive/2012/05/31/windows-8-release-preview-and-gdfs.aspx">Windows 8</a> Release Preview / RTM and <a href="http://blogs.msdn.com/b/chuckw/archive/2012/05/31/visual-studio-2012-release-candidate.aspx">Visual Studio 2012</a> RC / RTM. XINPUT 1.4 is supported on Windows RT as well.</p>
<p><strong>REDIST:</strong> For XInput 1.4 on Windows 8 and Windows RT, no redistribution is required since XInput 1.4 is included with the OS. For XInput 9.1.0 on Windows Vista, Windows 7, or WIndows 8, no redistribution is required since XInput 9.1.0 is included with the OS. For XInput 1.3 on any version of Windows, use the <a href="http://blogs.msdn.com/b/chuckw/archive/2010/09/08/not-so-direct-setup.aspx">legacy DirectX SDK REDIST</a>.</p>
<p>Since Xinput version 1.4 is not available on Windows 7, Win32 desktop games that support older versions of Windows can use either XInput 1.3 or the still supported older XInput 9.1.0 which is included in Windows Vista, Windows 7, and Windows 8.</p>
<ul>
<li>When building an application that is 'down-level' using headers in the Windows 8.0 SDK, be sure to explicitly select the correct 'minimum' <code>_WIN32_WINNT</code> value. For Windows 8, that is 0x0602 (which is the default when building code with Visual Studio 2012 and for all Windows Store apps). For Windows 7 use 0x0601, and for Windows Vista use 0x0600. Typically this is done as part of the project configuration via <em>Preprocessor Definitions</em>.</li>
</ul>
<p><em>If you set _WIN32_WINNT correctly and try building with the Windows 8.0 SDK version of the <code>xinput.h</code> header, you will be using XInput 1.4 if set to 0x0602, or XInput 9.1.0 otherwise. If using XInput 1.4, you should link with <code>XINPUT.LIB</code>. If using XInput 9.1.0, link with <code>XINPUT9_1_0.LIB</code> instead.</em></p>
<p>If your usage of XInput is limited to basic gamepad functionality via <code>XInputGetState</code> and <code>XInputSetState</code>, then XInput 9.1.0 may be all the functionailty you require and is supported on a broad range of Windows OSes without the need to use the DirectSetup redistribution package. A few things to note about XInput 9.1.0 are that <code>XInputGetCapabilities</code> returns a fixed set of values regardless of the attached device (notably including the subtype), and this version of XInput does not support headset audio functionality.</p>
<p>The following code will compile using all three versions of XInput:</p>
<pre class="scroll"><code class="cplusplus"> #include &lt;xinput.h&gt;<br /> <br /> XINPUT_STATE state;<br /> memset( &amp;state, 0, sizeof(XINPUT_STATE) );<br /> <br /> if( XInputGetState( 0, &amp;state ) == ERROR_SUCCESS )<br /> {<br /> // Controller is connected<br /> if ( state.Gamepad.wButtons &amp; XINPUT_GAMEPAD_A )<br /> {<br /> // Button A is pressed<br /> }<br /> <br /> XINPUT_VIBRATION motor;<br /> memset( &amp;motor, 0, sizeof(XINPUT_VIBRATION) );<br /> <br /> if ( state.Gamepad.bLeftTrigger &gt; XINPUT_GAMEPAD_TRIGGER_THRESHOLD )<br /> {<br /> motor.wLeftMotorSpeed = state.Gamepad.bLeftTrigger &lt;&lt; 8;<br /> }<br /> <br /> if ( state.Gamepad.bRightTrigger &gt; XINPUT_GAMEPAD_TRIGGER_THRESHOLD )<br /> {<br /> motor.wRightMotorSpeed = state.Gamepad.bRightTrigger &lt;&lt; 8;<br /> }<br /> <br /> XInputSetState( 0, &amp;motor );<br /> <br /> }<br /> else<br /> {<br /> // Controller is not connected, shouldn't recheck it for a few seconds<br /> }</code></pre>
<p>If using the XInput 1.3 solution, You should follow the instructions on <a href="http://msdn.microsoft.com/en-us/library/windows/desktop/ee663275.aspx">MSDN</a> to use the Windows 8.0 SDK headers and libraries where possible and explicitly link to the <code>DXSDK_DIR</code> for the headers where you need older versions to support older versions of Windows. (see "<a href="http://blogs.msdn.com/b/chuckw/archive/2012/03/22/where-is-the-directx-sdk.aspx">Where is the DirectX SDK?</a>").</p>
<p>In a future <a href="http://blogs.msdn.com/b/chuckw/archive/2012/05/03/xinput-and-xaudio2.aspx">post</a>, I'll address the details of using XInput 1.4's audio features, and how to implement similiar behavior down-level using XInput 1.3. XInput 9.1.0 doesn't support audio features.</p>
<p><strong>Note:</strong> The <a href="http://blogs.msdn.com/b/chuckw/archive/2010/06/15/windows-sdk-7-1.aspx">Windows 7.1 SDK</a> includes the <code>xinput.h</code> header and <code>xinput.lib</code> import library for the the Xinput 9.1.0 version as well. XInput 9.1.0 can be deployed on Windows XP using the legacy DirectX SDK's REDIST (aka DirectSetup).</p>
<p><strong>Windows&nbsp;Server:</strong> Note that XInput is not included in Windows Server 2012. XInput 9.1.0 is also not present on Windows Server 2008 or 2012.</p>
<p><strong>Xbox One Controller:</strong> The Xbox One controller is not supported by the XUSB21.SYS (Windows 7) or XUSB22.SYS (Windows 8.x) driver. There is a X1USB1.SYS driver <a href="http://majornelson.com/2014/06/05/pc-drivers-for-the-xbox-one-controller-available-now/">now available</a> which does support the Xbox One Controller for XINPUT when using a micro-USB cable--the Xbox One controller is not compatible with the <em>Xbox 360&nbsp;Wireless Receiver for Windows</em>.</p>
<p><strong>Windows 10:</strong> To continue to use XInput 1.4 with universal Windows apps, be sure to link with <code>xinputuap.lib</code> rather than <code>xinput.lib</code>. Alternatively, you can make direct use of the new <a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.gaming.input">Windows.Gaming.Input</a> API or the <a href="http://blogs.msdn.com/b/chuckw/archive/2014/09/05/directx-tool-kit-now-with-gamepads.aspx">GamePad</a> class in <a href="https://github.com/Microsoft/DirectXTK">DirectX Tool Kit</a>.</p>
<p><strong>Related:</strong> <a href="http://code.msdn.microsoft.com/XInput-Win32-Samples-cc25ce24">XInput Win32 Samples</a>, <a href="http://code.msdn.microsoft.com/windowsapps/Simple-XInput-Controller-77c4b8e5">Game controller sample</a></p>