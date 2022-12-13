---
title: Unity中NGUI ScrollView 遮罩粒子
date: 2022-12-13 21:27:03
tags: Unity
---

## 起因:

​		在工作中由于美术想使用粒子作为UI中的物件时，发现NGUI 中 Scrollview 视图组件 无法裁剪掉粒子渲染显示，google 搜索到了一位 韩国游戏开发者的 博客，故做个笔记！此方案不用适配裁剪区域。

## 方案：

shader(完整代码)

1.新建一个shader文件，将shader文件里面的内容替换如下:

```
Shader "Unlit/StencilMask"
{
    Properties
    {
        _MainTex ("Texture", 2D) = "white" {}
    }
    SubShader
    {
        Tags { "RenderType"="Opaque" }
        LOD 100
        
        Pass
        {
            Stencil 
            {
              Ref 2
              Comp always
              Pass replace
            }
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            // make fog work
            #pragma multi_compile_fog

            #include "UnityCG.cginc"

            struct appdata
            {
                float4 vertex : POSITION;
                float2 uv : TEXCOORD0;
            };

            struct v2f
            {
                float2 uv : TEXCOORD0;
                UNITY_FOG_COORDS(1)
                float4 vertex : SV_POSITION;
            };

            sampler2D _MainTex;
            float4 _MainTex_ST;

            v2f vert (appdata v)
            {
                v2f o;
                o.vertex = UnityObjectToClipPos(v.vertex);
                o.uv = TRANSFORM_TEX(v.uv, _MainTex);
                UNITY_TRANSFER_FOG(o,o.vertex);
                return o;
            }
            fixed4 frag (v2f i) : SV_Target
            {
                fixed4 col = tex2D(_MainTex, i.uv);
                UNITY_APPLY_FOG(i.fogCoord, col);
                return col;
            }
            ENDCG
        }
    }
}
```

​	关键代码:	（模板缓冲区）

​	Stencil 

​     {
​              Ref 2
​              Comp always
​              Pass replace
​     }

具体说明：https://docs.unity3d.com/2019.3/Documentation/Manual/SL-Stencil.html

2.制作一张遮罩图片(黑白图即可)



3.在unity 层级视图 中新建一个 NGUI UITextrue 组件GameObject 与 Scrollview 保持同一父节点，将上面的shader 拖挂到 UITextrue 组件 的 shader 字段中，将遮罩图 拖挂到 Textrue字段，调整UITextrue组件大小与 Scrollview 视图裁剪一样的大小。

4.调整 ParticleSystemRenderer(粒子对象) 的属性  

​	Renderer组中 Masking 属性为Visible inside Mak

​     

## 测试:

​	经过以上步骤，运行unity项目 打开相关的 Scrollview窗口即可看到效果。
