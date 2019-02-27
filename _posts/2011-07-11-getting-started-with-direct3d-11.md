---
layout: post
title: Getting Started with Direct3D 11
date: 2011-07-11 13:37
author: walbourn
comments: true
categories: [direct3d, Uncategorized]
---
So you've downloaded the latest <span style="text-decoration: line-through">DirectX SDK</span>Windows SDK and start digging through the various <a href="https://github.com/walbourn/directx-sdk-samples/tree/master/Direct3D11Tutorials">tutorials</a>, <a href="http://blogs.msdn.com/b/chuckw/archive/2013/09/20/directx-sdk-samples-catalog.aspx">samples</a>, and <a href="http://msdn.microsoft.com/en-us/library/ff476345.aspx">documentation</a>, and you are trying to get a handle on where to start learning Direct3D 11...

<em>The <a href="https://github.com/Microsoft/DirectXTK/wiki/Getting-Started">DirectX Tool Kit tutorials</a> and the <a href="https://blogs.msdn.microsoft.com/chuckw/2015/12/17/direct3d-game-visual-studio-templates-redux/">Direct3D Game VS Templates</a> are a good place to start. You should also take a look at the Introductory Graphics samples on <a href="https://github.com/Microsoft/Xbox-ATG-Samples#introductory-graphics">Xbox-ATG-Samples</a>.</em>

When looking at presentations, a good place to start is to review the Direct3D 10 material especially if coming from a background of knowing Direct3D 9. Direct3D 11 is just an extension of Direct3D 10.x, and everything you learn about Direct3D 10 is applicable. These two presentations together cover the basics of using the API, changes to debugging, comparisions to Direct3D 9, and critical performance information for understanding how to optimize the new API.

<em>Introduction to Direct3D 10</em> (<a href="http://download.microsoft.com/download/D/5/0/D503E831-718C-469D-A4FD-5953E31EDE69/Introduction to Direct3D 10 Course (SIGGRAPH 2007).zip">SIGGRAPH 2007</a>)

<em>Windows to Reality: Getting the Most out of Direct3D 10 Graphics in Your Games</em> (<a href="https://msdnshared.blob.core.windows.net/media/2017/11/Windows-to-Reality-Getting-the-Most-out-of-Direct3D-10-Graphics-in-your-Games.zip">Gamefest 2007</a>)

After you are up-to-speed on Direct3D 10, take a little side journey to read up on the changes made for Direct3D 10.1, particularly the concept of 'feature levels'.

<em>The Evolving Windows Gaming Platform</em> (<a href="https://msdnshared.blob.core.windows.net/media/2017/11/The-Evolving-Windows-Gaming-Platform.zip">GDC 2008</a>)

From there you are fully prepared for the 'delta' covered in these two presentations on Direct3D 11 proper:

<em>Introduction to the Direct3D 11 Graphics Pipeline</em> (<a href="https://msdnshared.blob.core.windows.net/media/2017/11/Introduction-to-the-Direct3D-11-Graphics-Pipeline.zip">Gamefest 2008</a>)

<em>DirectX 11 Technology Update</em> (<a href="https://msdnshared.blob.core.windows.net/media/2017/11/DirectX-11-Technology-Update.zip">Gamefest 2010</a>)

For additional material, see: <a href="http://blogs.msdn.com/b/chuckw/archive/2012/06/20/direct3d-feature-levels.aspx">Feature Levels</a>, <a href="http://blogs.msdn.com/b/chuckw/archive/2010/07/19/direct3d-11-tessellation.aspx">Tessellation</a>, <a href="http://blogs.msdn.com/b/chuckw/archive/2010/07/14/directcompute.aspx">DirectCompute</a>, <a href="http://blogs.msdn.com/b/chuckw/archive/2010/08/23/direct3d-11-multithreading.aspx">Multithreading</a>, <a href="http://blogs.msdn.com/b/chuckw/archive/2012/05/04/direct3d-11-textures-and-block-compression.aspx">Textures and Block Compression</a>, <a href="http://blogs.msdn.com/b/chuckw/archive/2012/05/07/hlsl-fxc-and-d3dcompile.aspx">HLSL</a>, <a href="http://msdn.microsoft.com/en-us/library/windows/desktop/gg615082.aspx">WARP</a>, and <a href="http://blogs.msdn.com/b/chuckw/archive/2012/10/24/effects-for-direct3d-11-update.aspx">Effects</a>.

<strong>Math: </strong>Since Direct3D 11 does not have the 'fixed-function' graphics pipeline of Direct3D 9, the choice of graphics math conventions (left-handed vs. right-handed, row-major vs. column-major matrices, etc.) is entirely up to the developer. <a href="http://blogs.msdn.com/b/chuckw/archive/2012/03/27/introducing-directxmath.aspx">DirectXMath</a> can be used to create both Direct3D-style "Left-Hand Coordinate" transformations as well as OpenGL-style "Right-Handed Coordinate" transformations using a row-major matrix convention which can be used directly with row-major shaders or transposed to use column-major shaders. Note that the <a href="http://go.microsoft.com/fwlink/?LinkId=248929">DirectX Tool Kit</a> and <a href="http://blogs.msdn.com/b/shawnhar/archive/2013/01/08/simplemath-a-simplified-wrapper-for-directxmath.aspx">SimpleMath</a> uses the XNA Game Studio convention of "Right-handed, row-major".

<strong>Debugging:</strong> <a href="http://blogs.msdn.com/b/chuckw/archive/2012/08/15/visual-studio-2012-and-windows-8-0-sdk-rtm-are-now-available.aspx">Visual Studio 2012 / Windows 8.0 SDK</a> / <a href="http://blogs.msdn.com/b/chuckw/archive/2013/10/18/visual-studio-2013-and-windows-8-1-sdk-rtm-are-now-available.aspx">Visual Studio 2013 / Windows 8.1 SDK</a> (and the <a href="http://blogs.msdn.com/b/chuckw/archive/2012/03/22/where-is-the-directx-sdk.aspx">legacy DirectX SDK</a>) installs a number of '<a href="http://blogs.msdn.com/b/chuckw/archive/2012/11/30/direct3d-sdk-debug-layer-tricks.aspx">debugging</a>' features for development purposes including the "Debug" layers (<code>D3D10SDKLayers.DLL</code>, <code>D3D11SDKLayers.DLL</code>, <code>D3D11_1SDKLayers.DLL</code>, <code>DXGIDEBUG.DLL</code>, <code>D2D1Debug1.DLL</code>) and "Reference" devices (<code>D3D10REF.DLL</code>, <code>D3D11REF.DLL</code>, and <code>D3DREF9.DLL</code>). These cannot be used in released products or Windows Store app submissions as they are not present on end-user (i.e. not developer/test) machines. This means that 'release production' builds of your application cannot make use of <code><a href="http://msdn.microsoft.com/en-us/library/windows/desktop/ff476881.aspx#Debug">D3D11_CREATE_DEVICE_DEBUG</a></code>, <code>D3D11_CREATE_DEVICE_SWITCH_TO_REF</code>, <code>D3D11_CREATE_DEVICE_DEBUGGABLE</code>, <code><a href="http://msdn.microsoft.com/en-us/library/windows/desktop/ff476328.aspx">D3D_DRIVER_TYPE_REFERENCE</a></code>,<a href="http://msdn.microsoft.com/en-us/library/windows/desktop/hh780351.aspx"> <code>DXGIGetDebugInterface</code></a>, <code><a href="http://msdn.microsoft.com/en-us/library/windows/desktop/ee794277.aspx">D2D1_FACTORY_OPTIONS.debugLevel</a> != D2D1_DEBUG_LEVEL_NONE</code>, or <code><a href="http://msdn.microsoft.com/en-us/library/windows/desktop/bb172547.aspx">D3DDEVTYPE_REF</a></code>.

On Windows 10, the debug layer is not installed by any SDK. Instead you enable the <em>Graphics Tools</em> Windows Optional Feature. See <a href="https://blogs.msdn.microsoft.com/vcblog/2015/03/31/visual-studio-2015-and-graphics-tools-for-windows-10/">this blog post</a>.

<strong>Update:</strong> For texture processing, see <a href="http://go.microsoft.com/fwlink/?LinkId=248926">DirectXTex</a>. For sprites, texture loading, 'basic' shaders, geometry shapes, a simple font system, and a number of other utilities see <a href="http://go.microsoft.com/fwlink/?LinkId=248929">DirectX Tool Kit</a>. For mesh processing such as computing vertex normals and vertex cache optimization, see <a href="http://go.microsoft.com/fwlink/?LinkID=324981">DirectXMesh</a>. All versions of <a href="http://blogs.msdn.com/b/chuckw/archive/2013/08/21/living-without-d3dx.aspx">D3DX</a> including D3DX11 are deprecated.

<strong>Note:</strong> Be sure to read "<a href="https://blogs.msdn.microsoft.com/chuckw/2015/08/05/where-is-the-directx-sdk-2015-edition/">Where is the DirectX SDK?</a>" to get the latest news on developing for DirectX, particularly if you are using Windows 8.x, VS 2012 or 2013. There is a version of the original <em>DirectX SDK</em> Win32 desktop Direct3D 11 tutorials updated for use with VS 2012/2013 without the requiring the legacy DirectX SDK on the <a href="http://code.msdn.microsoft.com/windowsdesktop/Direct3D-Tutorial-Win32-829979ef">MSDN Code Gallery</a> along with a new version of <a href="http://blogs.msdn.com/b/chuckw/archive/2013/09/14/dxut-for-win32-desktop-update.aspx">DXUT</a>, <a href="http://go.microsoft.com/fwlink/p/?LinkId=271568">Effects 11</a>, and many of the <a href="http://blogs.msdn.com/b/chuckw/archive/2013/09/20/directx-sdk-samples-catalog.aspx">samples</a> from the DirectX SDK.

<em>If you are primarily interested in learning DirectX 11 for Windows Store app (a.k.a. Metro style app) development, <a href="http://msdn.microsoft.com/en-us/library/windows/apps/hh452744.aspx">MSDN</a> has a lot of material and there are a number of samples on the <a href="http://code.msdn.microsoft.com/windowsapps/">Windows Code Gallery</a> including the Windows Store app equivalent of the original DirectX SDK <a href="http://code.msdn.microsoft.com/windowsapps/Direct3D-Tutorial-Sample-08667fb0">Direct3D 11 tutorials</a>.</em>

<strong>Related</strong>: <a href="http://blogs.msdn.com/b/chuckw/archive/2013/08/21/living-without-d3dx.aspx">Living Without D3DX</a>, <a href="http://blogs.msdn.com/b/chuckw/archive/2014/04/07/book-recommendations.aspx">Book Recommendations</a>, <a href="http://blogs.msdn.com/b/chuckw/archive/2014/02/05/anatomy-of-direct3d-11-create-device.aspx">Anatomy of Direct3D 11 Create Device</a>, <a href="http://blogs.msdn.com/b/chuckw/archive/2012/11/30/direct3d-sdk-debug-layer-tricks.aspx">Direct3D SDK Debug Layer Tricks</a>, <a href="https://blogs.msdn.microsoft.com/chuckw/2015/12/17/direct3d-game-visual-studio-templates-redux/">Direct3D Win32 Game Visual Studio template</a>