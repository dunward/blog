---
layout: post
title:  Unity yaml과 TagManager
date:   2022-05-03 21:12:03 +0900
categories: unity
---
익명 카톡방에서 물어본 이슈인데 유니티 yaml과 관련해서 설명하기 좋을듯해서 블로그에 정리

유니티 Scene에서는 내부 GameObject를 yaml로 아래와같이 저장합니다.

## Scene yaml
```
GameObject:
  m_ObjectHideFlags: 0
  m_CorrespondingSourceObject: {fileID: 0}
  m_PrefabInstance: {fileID: 0}
  m_PrefabAsset: {fileID: 0}
  serializedVersion: 6
  m_Component:
  - component: {fileID: 281801016}
  m_Layer: 0
  m_Name: GameObject
  m_TagString: Character
  m_Icon: {fileID: 0}
  m_NavMeshLayer: 0
  m_StaticEditorFlags: 0
  m_IsActive: 1
```
여기서 m_TagString을 보면 알수있다시피 Tag정보를 String으로 관리하고있는데 ProjectSettings/TagManager.asset에서 해당 TagString을 찾지 못할경우엔 Scene yaml에서 해당 TagString을 Undefined로 바꿉니다. 이때 변경된 씬을 저장하지않고, 원래 태그명을 그대로 추가해준다면 Scene을 다시 로드하면 tag가 그대로 유지되어있는 모습을 볼 수 있을겁니다.

## TagManager.asset
```
TagManager:
  serializedVersion: 2
  tags:
  - Undefined
  - Character
```

내부적으로 유니티에서는 Scene 로드시에 TagString을 한번씩 TagManager와 대조를 해보고, 해당 태그가 invalid 할경우에는 TagString을 Undefined로, 만약 해당 태그가 없다면 Undefined 태그도 새로 추가하도록 구현되어있다고 생각할 수 있습니다.

보통 이런문제가 생기는 경우는 유니티의 프로젝트 저장개념에 대해서 친숙하지 않을때 자주 겪을법한데, 코드를 저장하고 유니티에서 변화를 감지하고 auto-complie 모드라고 해서 새롭게 cs파일을 compile해도 프로젝트가 저장되는것이 아닙니다.

개인적으로는 유니티에서 저장되는걸 크게 세가지로 분류하고 있습니다.

- Code
- Asset
- Project

## Code
Code는 위에서 간략하게 설명했다시피 일반적으로 vsc 혹은 vs에서 수정시, auto-compile이던 아니던 가장 자주 저장되는것중 하나입니다. 별다른 설명을 하지 않아도 괜찮을거라 생각듭니다.

## Asset
Scene, Prefab 등등 이러한것들을 통틀어서 asset이라 칭하겠습니다. 보통 저장을 어떤식으로 하시는지는 모르겠으나 Scene은 변경점이 있다면 보통 Ctrl+S로 저장하기 때문에 큰 문제는 없고, prefab은 auto-save 기능을 사용하지 않을때는 수동으로 save 해줍니다. auto-save의 조건에 대해서는 자세히 모르기때문에 추후에 알아보는것도 괜찮겠네요.

## Project
위에 두개 저장한거면 끝이 아닌가? 싶을수있는데, 위에 Scene의 저장방법과 똑같습니다. 유니티 윈도우에 포커스가 잡힌상태로 Ctrl+S하는게 다입니다. File-Save로도 가능합니다. 이게 Scene을 저장하는 방식이기도 한데, Scene을 저장하면서 기타 유니티 프로젝트에서 변경했던것들도 같이 이때 저장됩니다. ProjectSettings directory안에 있는 asset들은 Scene의 저장 프로세스에 포함되어있다고 생각하시면 될거같습니다.

뭔가 내용자체가 두루뭉실하고 설명이 이상한거같은데 결론은 IDE나 Code Editor에서 저장했다고 유니티 프로젝트가 저장됬다고 안심하지말고, 유니티에서 직접 한번씩 저장해주는게 좋습니다.