Asset文件本身：
```
%YAML 1.1 // 版本号
%TAG !u! tag:unity3d.com,2011: // tag 不用管
--- !u!1001 &100100000 // 这个叫objectId，刚刚的guid我们称之为FileID,前面的1001标识下面是一个什                           么类型，是一个枚举；下面同理
Prefab:
  m_ObjectHideFlags: 1
  serializedVersion: 2
  m_Modification:
    m_TransformParent: {fileID: 0}
    m_Modifications: []
    m_RemovedComponents: []
  m_ParentPrefab: {fileID: 0}
  m_RootGameObject: {fileID: 1787787853067516}
  m_IsPrefabParent: 1
--- !u!1 &1787787853067516
GameObject:                 //这里面的值都是它对应的Inspector上的属性(有些需要打开Debug才能看到)
  m_ObjectHideFlags: 0
  m_PrefabParentObject: {fileID: 0}
  m_PrefabInternal: {fileID:  }
  serializedVersion: 5
  m_Component:            //描述了当前go下面都包含了哪些component
                          //注意这个fileID，它描述了这些component的objectid，可以看到它和下面的是一一对应的。通过这个id把下面的信息填充到这个地方来
  - component: {fileID: 4227263950436128}
  - component: {fileID: 33565322050093970}
  - component: {fileID: 23555425417039670}
  m_Layer: 0
  m_Name: Pistol
  m_TagString: Untagged
  m_Icon: {fileID: 0}
  m_NavMeshLayer: 0
  m_StaticEditorFlags: 0
  m_IsActive: 1
--- !u!4 &4227263950436128
Transform:
  m_ObjectHideFlags: 1
  m_PrefabParentObject: {fileID: 0}
  m_PrefabInternal: {fileID: 100100000}
  m_GameObject: {fileID: 1787787853067516}
  m_LocalRotation: {x: 0, y: 0, z: 0, w: 1}
  m_LocalPosition: {x: 0, y: 0, z: 0}
  m_LocalScale: {x: 1, y: 1, z: 1}
  m_Children: []
  m_Father: {fileID: 0}
  m_RootOrder: 0
  m_LocalEulerAnglesHint: {x: 0, y: 0, z: 0}
--- !u!23 &23555425417039670
MeshRenderer:
  m_ObjectHideFlags: 1
  m_PrefabParentObject: {fileID: 0}
  m_PrefabInternal: {fileID: 100100000}
  m_GameObject: {fileID: 1787787853067516}
  m_Enabled: 1
  m_CastShadows: 1
  m_ReceiveShadows: 1
  m_DynamicOccludee: 1
  m_MotionVectors: 1
  m_LightProbeUsage: 1
  m_ReflectionProbeUsage: 1
  m_RenderingLayerMask: 4294967295
  m_Materials:
  - {fileID: 2100000, guid: cdd1dda890977a24b8597b566bc084de, type: 2}
  m_StaticBatchInfo:
    firstSubMesh: 0
    subMeshCount: 0
  m_StaticBatchRoot: {fileID: 0}
  m_ProbeAnchor: {fileID: 0}
  m_LightProbeVolumeOverride: {fileID: 0}
  m_ScaleInLightmap: 1
  m_PreserveUVs: 0
  m_IgnoreNormalsForChartDetection: 0
  m_ImportantGI: 0
  m_StitchLightmapSeams: 0
  m_SelectedEditorRenderState: 3
  m_MinimumChartSize: 4
  m_AutoUVMaxDistance: 0.5
  m_AutoUVMaxAngle: 89
  m_LightmapParameters: {fileID: 0}
  m_SortingLayerID: 0
  m_SortingLayer: 0
  m_SortingOrder: 0
--- !u!33 &33565322050093970
MeshFilter:
  m_ObjectHideFlags: 1
  m_PrefabParentObject: {fileID: 0}
  m_PrefabInternal: {fileID: 100100000}
  m_GameObject: {fileID: 1787787853067516}
  m_Mesh: {fileID: 4300000, guid: d9951ecbd04e28f4db3eb8ddb3775842, type: 3}

```