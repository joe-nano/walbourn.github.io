---
layout: post
title: DirectXTex and Effects 11 Update
date: 2013-08-15 11:57
author: walbourn
comments: true
categories: [codeplex, direct3d, directcompute, Uncategorized]
---
<h1>DirectXTex</h1>
<p>It has been a busy summer which has resulted in "version 1.2" of the DirectXTex texture processing library. The focus of this release has been on improving Block Compression support. The biggest new feature is the integration of the DirectCompute 4.0 accelerated BC6H / BC7 texture compression codecs from the <a href="http://go.microsoft.com/fwlink/?LinkId=254494">BC6HBC7EncoderCS</a> sample. The latest <code>texconv</code> command-line tool will attempt to use the DirectCompute version when compressing for BC6H / BC7 when running on a system with a DirectCompute 4.0 capable hardware device (aka a Feature Level 10.0 or 10.1 video card with the optional DirectCompute feature or a Feature Level 11.x video card). This is a huge increase in performance compared to the original D3DX11 BC6H / BC7 software codecs.</p>
<p>For example, on my primary development system a 8K by 4K texture with mips&nbsp;took 25 minutes to compress using the multithreaded version of the D3DX11 BC6H / BC7 software codec across six hyper threaded CPU cores, which only took about 2.5 minutes using DirectCompute 4.0. I gave up on timing the single-threaded D3DX11 BC6H / BC7 software codec after an hour.</p>
<p><strong>Note:</strong> 2.5 minutes is still a significant compress time for a single texture so future work here will focus on implementing a new faster algorithm using DirectCompute 5.0.</p>
<p>The latest release is available on <a href="https://directxtex.codeplex.com/">CodePlex</a>&nbsp;and <a href="https://github.com/Microsoft/DirectXTex">GitHub</a>. The documentation on <a href="https://directxtex.codeplex.com/documentation">Codeplex</a> has also been updated.</p>
<p><strong>August 13, 2013</strong></p>
<ul>
<li>DirectCompute 4.0 BC6H/BC7 compressor integration</li>
<li>texconv utility uses DirectCompute compression by default for BC6H/BC7, -nogpu disables use of DirectCompute</li>
</ul>
<p><strong>August 1, 2013</strong></p>
<ul>
<li>Support for BC compression/decompression of non-power-of-2 mipmapped textures</li>
<li>Fixes for BC6H / BC7 codecs to better match published standard</li>
<li>Fix for BC4 / BC5 codecs when compressing RGB images</li>
<li>Minor fix for the BC1-3 codec</li>
<li>New optional flags for ComputeMSE to compare UNORM vs. SNORM images</li>
<li>New WIC loading flag added to control use of WIC metadata to return sRGB vs. non-sRGB formats</li>
<li>Code cleanup and /analyze fixes</li>
<li>Project file cleanup</li>
<li>Texconv utility uses parallel BC compression by default for BC6H/BC7, -singleproc disables multithreaded behavior</li>
</ul>
<p><strong>July 1, 2013</strong></p>
<ul>
<li>VS 2013 Preview projects added</li>
<li>SaveToWIC functions updated with new optional setCustomProps parameter</li>
</ul>
<p><strong>Related:</strong> <a href="http://blogs.msdn.com/b/chuckw/archive/2013/06/15/directxtex-update.aspx">DirectXTex Update (June 2013)</a>,&nbsp;<a href="http://blogs.msdn.com/b/chuckw/archive/2011/10/28/directxtex.aspx">DirectXTex (October 2011)</a></p>
<p>&nbsp;</p>
<h1>Effects11</h1>
<p>In other news, the <a href="http://msdn.microsoft.com/en-us/library/windows/desktop/ff476136.aspx">Effects 11 library</a> now has a home on <a href="https://fx11.codeplex.com/">CodePlex</a>&nbsp;and on <a href="https://github.com/Microsoft/FX11">GitHub</a>. Be sure to read my original <a href="http://blogs.msdn.com/b/chuckw/archive/2012/10/24/effects-for-direct3d-11-update.aspx">Effects for Direct3D 11 Update</a> post for details on this library.</p>
<p><strong>July 16, 2013 (11.07)</strong></p>
<ul>
<li>Added VS 2013 Preview project files</li>
<li>Cleaned up project files</li>
<li>Fixed a number of /analyze issues</li>
</ul>
<p><strong>June 13, 2013 (11.06)</strong></p>
<ul>
<li>Added GetMatrixPointerArray, GetMatrixTransposePointerArray, SetMatrixPointerArray, SetMatrixTransposePointerArray methods</li>
<li>Reverted back to BOOL in some cases because sizeof(bool)==1, sizeof(BOOL)==4</li>
<li>Some code-cleanup: minor SAL fix, removed bad assert, and added use of override keyword</li>
</ul>
<p><strong>February 22, 2013 (11.05)</strong></p>
<ul>
<li>Cleaned up some warning level 4 warnings</li>
</ul>
<p><strong>November 6, 2012 (11.04)</strong></p>
<ul>
<li>Added IUnknown as a base class for all Effects 11 interfaces to simplify use in managed interop scenarios, although the lifetime for these objects is still based on the lifetime of the parent ID3DX11Effect object. Therefore reference counting is ignored for these interfaces.</li>
<ul>
<li>ID3DX11EffectType, ID3DX11EffectVariable and derived classes, ID3DX11EffectPass, ID3DX11EffectTechnique, and ID3DX11EffectGroup</li>
</ul>
</ul>
<p>&nbsp;</p>