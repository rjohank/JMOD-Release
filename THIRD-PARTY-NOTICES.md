# Third-party notices

JMOD includes or links the libraries below. Each is used under its own license.

| Library | License | Copyright |
|---|---|---|
| [Microsoft Detours](https://github.com/microsoft/Detours) | MIT | Copyright (c) Microsoft Corporation |
| [Dear ImGui](https://github.com/ocornut/imgui) | MIT | Copyright (c) 2014-2026 Omar Cornut |
| [nlohmann/json](https://github.com/nlohmann/json) | MIT | Copyright (c) 2013-2025 Niels Lohmann |
| [Barbatos Neural Sharpening](https://github.com/BarbatosAWLS/Reshade-Shaders) | MIT | Copyright (c) Barbatos |
| [NVIDIA Streamline](https://github.com/NVIDIA-RTX/Streamline) (headers) | MIT | Copyright (c) 2023 NVIDIA CORPORATION |
| [Vulkan-Headers](https://github.com/KhronosGroup/Vulkan-Headers) | Apache-2.0 OR MIT | Copyright 2015-2026 The Khronos Group Inc. |
| [gtl](https://github.com/greg7mdp/gtl) | Apache-2.0 | Copyright its authors; includes parts under their own notices from Google, Inc. (2014) and Quarkslab (2017) |
| [frozen](https://github.com/serge-sans-paille/frozen) | Apache-2.0 | Copyright its authors |
| [NVIDIA DLSS SDK](https://github.com/NVIDIA/DLSS) (headers) | NVIDIA RTX SDKs License | Copyright NVIDIA Corporation |

## MIT License (Detours, Dear ImGui, nlohmann/json, Barbatos Neural Sharpening, Streamline)

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## Apache License 2.0 (Vulkan-Headers, gtl, frozen)

Licensed under the Apache License, Version 2.0. You may obtain a copy of the
License at <http://www.apache.org/licenses/LICENSE-2.0>. Unless required by
applicable law or agreed to in writing, software distributed under the License
is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.

## NVIDIA DLSS SDK

Used under the NVIDIA RTX SDKs License. The license is available at
<https://github.com/NVIDIA/DLSS/blob/main/LICENSE.txt>.

## Intel XeSS

`jmod\xefg\libxess_fg.dll` is Intel's XeSS Frame Generation library, redistributed
unmodified. Copyright Intel Corporation. It is subject to the license of the
Intel XeSS SDK: <https://github.com/intel/xess>.

## NVIDIA runtime files

NVIDIA's runtime files (the Streamline DLLs, `nvngx_dlss.dll`, `nvngx_dlssg.dll`,
`nvngx_dlssnr.dll` and similar) are **not included** in JMOD. You supply them
yourself, and they stay under NVIDIA's own license terms.
