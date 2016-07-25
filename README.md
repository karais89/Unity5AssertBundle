# README #

## Unity5 AssertBundle Example ##

http://dongin2009.blog.me/

## 유니티 5.0의 에셋 번들에 대해 공부해보자 part 1. 탐구편 ##

1. 에셋 번들은 무엇일까?

Asset + Bundle

Asset = 프리팹, 텍스쳐, 재질, 스크립트, 쉐이더등의 자원들..

Bundle = 묶음

에셋번들은? 에셋의 묶음

2. 에셋 번들의 쓰임과 장점

에셋 번들은 에셋의 묶음이고 원하는 에셋들을 따로 묶어 자장할 수 있습니다.

에셋 번들은 서버에 따로 저장해 놓고 www 통신을 통하여 다운 받을 수 있습니다.

그렇기 때문에 몇 가지 확실한 장점이 있습니다.

**앱스토어에 등록할 최초배포버전의 용량을 줄일 수 있다.**

에셋 번들은 배포시에 포함 시킬 필요가 없습니다. 서버에서 다운받아 사용할 수 있기 때문에 배포버전을 경량화 시킬 수 있습니다.

어느 조사에 의하면 용량이 50MB 이하일때 페이지의 노출 수 대비 다운로드 수가 가장 많다고 합니다.

**게임의 업데이트에 용이합니다.**

에셋번들은 번들별로 CRC, 버전 등의 정보를 가지고 있습니다.

그렇기 때문에 업데이트시에 버전 정보가 다른 에셋 번들들만 업데이트해주면 되니 업데이트에도 유용하게 사용할 수 있습니다.

[유니티 에셋번들 메뉴얼](http://docs.unity3d.com/Manual/AssetBundlesIntro.html)

## 유니티 5.0의 에셋 번들에 대해 공부해보자 part 2. 생성과 빌드편 ##

인스펙터 창 우측 하단의 AssetLabels로 에셋번들을 설정 할 수 있다.

중앙의 긴 리스트 상자에는 에셋번들의 이름을 입력합니다. 가장 중요하고 가장 많이 쓰입니다.

우측의 리스트 상자에는 변형 에셋번들을 만드는 칸입니다. 

꼬리표는 말그래도 이 에셋의 종류를 표시할 수 있는 기능입니다. (캐릭터인지, 이펙트인지, 혹은 터레인인지 하는 꼬리표를 복수로 체크하여 표시)

하지만 결정적으로 번들은 이 꼬리표를 공유하지 않습니다. 번들에는 기록되지 않는 것 같습니다.

에셋번들의 특이점

1. 에셋번들에 등록하는 이름에는 대문자를 포함할 수 없습니다. 대문자를 입력해도 전부 소문자로 자동 변경됩니다.
2. 에셋번들을 등록할때 이름을 폴더이름/에셋이름과 같이 등록하면 리스트 박스에 하위요소로 분류되면 에셋번들 빌드시에도 에셋번들이 해당 폴더로 위치합니다.

에셋번들에 포함할때의 방법 두가지

1. 등록하려는 프리팹들이 포함된 폴더 자체를 assetbundle에 포함합니다. 이 경우엔 폴더 하위 오브젝트들이 전부 등록됩니다.
2. 오브젝트들만 따로 클릭하여 assetbundle에 포함시켜도 됩니다. 다중 선택을 통해 한번에 등록할 수 있습니다.

유니티 4.x와의 차이점과 함수 이용팁

1. 원래 4.x 버전까지는 BuildPipeline.BuildAssetBunlde(메인에섯, 에셋배열[], "경로", BuildAssetBunldeOption, BuildTarget) 등을 요구했습니다.

   5.x에서 메인 에셋은 0번째로 등록된 에셋으로 설정되게끔 바뀌었고 에셋 배열은 더이상 Selection 기능을 이용하여 수집하지 않아도 자동적으로 수집됩니다.

2. 위의 4.x 함수의 매개변수 중 BuildAssetBundleOptions에 중요한 기능 하나가 있었습니다. 

   BuildAssetBundleOptions.CollectDependencies 이라는 기능인데 이것을 옵션으로 넣어주지 않으면 오브젝트가 제대로 저장되지 않았습니다.

   Prefab은 텍스쳐와 메테리얼, 스크립트 등과 유동적으로 상호작용하기 때문에 이 옵션을 넣어줘야만 프리팹에 종속된 자원들을 함께 저장했습니다.

   하지만 Unity 5.x 버전에서는 이 옵션이 Default 값으로 들어가 자동적으로 기능하게 되었습니다.

3. 만일 안드로이드 등 다른 플랫폼에세 사용할 시에 BuildAssetBundles() 함수의 오버로드 함수 중 BuildTarget을 요구하는 함수가 있습니다.

   이 함수에 BuildTarget target = BuildTarget.Android 이런식으로 옵션을 넣어주면 에러를 잡을 수 있습니다. 중요!!

## 유니티 5.0의 에셋 번들에 대해 공부해보자! part 3. 다운로드와 로딩편 ## 

에셋 번들 다운로드하기

에셋번들 다운로드 방식

1. 캐싱(caching)
2. 논캐싱(non-caching)

캐싱은 빌드시에 Unity3D Cache 폴더에 다운받은 에셋 번들을 저장합니다. 버전별로 저장해줍니다.

반대로 논캐싱은 저장하지 않겠지요. 

게임을 만들때는 캐싱 기법을 사용하는 것이 맞는 것 같습니다.

다운로드 서버는 bitnami등을 이용해 로컬 서버로 돌리던지 하시면 됩니다.

Cubes폴더의 내용물을 삭제해도 에셋번들을 로드하면 정상적으로 실행됩니다. (에셋에 대한 의존성이 알아서 잘 작동합니다)

## 에셋 번들을 다룰 때의 주의점에 대하여 알아보자! ## 

에셋번들 매니저를 만들어 볼까 생각하고 있습니다.

최대한 깔끔하고 편리하되, 기초저인 부분들만 구현해보려고 합니다.

매니저 구현에 앞서 에셋번들의 특징들에 대하여 간단하게 알아보려고 합니다.

무슨 클래스 혹은 기능들을 매니져화 시키던지 항상 기초적인 설계를 하고 들어가야 하기 때문에 설계 이전에 특징들을 한번 훑고 지나가는 것이 설계에 많은 도움이 되리라 생각됩니다.

### 에셋 번들의 다중 로딩 ###

```
WWW www = WWW.LoadFromCacheOrDownload(url, version);
```

저희는 보통 이 함수를 이용해 AssetBundle의 Caching 유무를 검사합니다.

캐싱되어 있다면 Cache폴더에 번들을 불려오고, 캐싱되어 있지 않다면 서버에서 받아온 번들을 Caching 한 후에 불러오게 되지요.

```
AssetBundle ab = www.assetBundle;
ab.Unload(false);
```

더불어 꼭 에셋 번들을 사용한 후에는 꼭 위처럼 Unload 함수를 실행해줘야 합니다.

그런데 Unload 함수를 실행해주지 않은 채 다른 AssetBundle 클래스로 같은 에셋 번들을 참조한다면 어떻게 될까요? (AssetBunlde 클래스는 참조형(Class형)입니다.)

4.x 버전까지는 에러 메시지가 출력되며 null 값이 참조되었다고 합니다. Unload() 함수로 언로드된 번들만이 참조될 수 있었죠.

하지만 5.x 버전에서는 한 에셋 번들을 여러 에셋번들 클래스로 참조하는 것이 가능합니다.

```
AssetBundle ab1 = www.assetbundle;
AssetBundle ab2 = www.assetbundle;
```

이런 식의 참조가 가능하다는 것이죠. 물론 각각의 참조에서 데이터를 뽑아낼 수 있습니다.

하지만 여전히 주의하셔야 할 점이 하나 있습니다.

원래 Unity3d는 c# 기반의 가비지컬렉터를 가지고 있기 때문에 따로 Unload() 할 필요가 없습니다.

참조형(class)의 경우엔 Heap에 저장된 데이터에 대한 모든 참조가 끊기면 가비지컬렉팅의 대상이 되고, 가비지 컬렉터가 메모리를 털어주기 때문입니다.

따로 인스턴스를 돌면서 메모리를 터는 함수를 실행할 필요가 없습니다.

하지만 이 AssetBundle클래스는 c++ 기반의 클래스로서 Unload 함수를 가지고 있습니다.

거기에 중요한 것이 WWW 하위의 AssetBundle 클래스는 절대적으로 한개의 인스턴스만을 가진다는 것입니다.

즉 위의 코드에서는 ab1과 ab2는 각각 다른 인스턴스를 참조하는 것이 아닌 하나의 인스턴스를 같이 참조하는 것입니다

고로 c#과 c++ 양쪽의 경우로 나누어 Unload() 함수를 실행한 이후의 결과를 예상해보자면, C#의 경우는 unload() 함수를 실행한 참조형 본인만 null을 참조하게 될 뿐 데이터는 보전될 것입니다.

다른 참조형이 여전히 같은 메모리 데이터를 물고 있기 때문이죠.

하지만 c++는 unload() 함수를 실행한 순간 메모리가 털릴 것입니다. 한마디로 이후에 메모리에 접근하는 다른 참조형들은 엑세스 에러를 리턴할 수 밖에 없다는 거죠

텅 빈 데이터를 참조하고 있는 상황이 만들어지게 됩니다.

때문에 에셋번들 매니저가 필요한 것입니다. unload() 함수의 복수호출을 막기 위해서지요.

물론 에셋 번들의 unload()전에 체크하는 방법을 사용해도 좋겠지요.

### Unload(bool)의 매개변수 형태

assetbundle.Unload() 함수는 1개의 bool값을 매개변수로 요구합니다.

false 값을 입력하면 기존에 불려왔던 데이터는 유지되되 assetbundle만 언로드되며, true 값을 입력하면 이 에셋번들에서 불러왔던 데이터까지 죄다 날려버립니다.

### 에셋 번들과 에셋의 Load 시간

실험을 해봤는데 Time.realtimeSinceStartup을 통하여 캐싱 전후의 Load 시간을 비교해 봤습니다.

당연히 용량이 큰 에셋 번들이 캐싱전, 후의 로드 시간이 클 수 밖에 없었습니다.

하지만 용량이 큰 번들이라도 캐싱되어 Cache 폴더에 저장된 이후에는 불러오는 시간이 정말 많이 감소되었습니다.

그리고 assetbundle.LoadAsset() 함수로 에셋을 불러오는데 걸리는 시간 역시 측정해봤는데 키성 전후의 시간차이는 나지 않으나, 용량이 커짐에 따라 은근히 시간을 많이 잡아먹습니다.

로딩씬에서 로드된 채로 사용하는 편이 좋겠고, 리소스의 용량 다이어트도 반드시 필요합니다.

또한 에셋번들을 잘 분류하는 것도 중요할 것 같습니다.

최근 유행하는 카드형 MMORPG류의 수백종의 캐릭터의 모델링, 텍스쳐, 메테리얼을 한 에셋번들에 담는다면...

무식하게 큰 돼지 에셋번들이 나오겠고, 불러오는데 시간이 꽤나 걸리겠죠??

더군다나 에셋 번들이 크기가 커지면 크래쉬가 날 확률도 있다고 하니..

사운드 BGM과 Effect로 묶는다던가. UI는 Scene 단위로 나눈다던가 하는 작업이 필요해 보입니다.

### 에셋 번들의 버전 정보와 저장형식

에셋 번들을 사용할 때 분명히 url과 버전 두가지 정보를 사용한다고 말씀드렸습니다.

헌데 에셋 번들을 만들때 버전을 설정해주는 아무런 매개변수도 입력하지 않죠

대체 어디서 버전을 설정하는 걸까? 라고 초반에 정말 많이 궁금해 했었습니다.

그러다 어느날 광명이 비치듯 깨닫게 되었죠 ㅎㅎ

사실 에셋 번들의 데이터에는 버전 정보가 포함되지 않습니다.

오히려 에셋번들의 빌드시가 아닌 에셋번들을 캐싱할때 결정되는 것입니다.

애초에 url 주소에 저장된 에셋 번들을 캐싱할 때, 캐싱된 에셋 번들은 그 에셋 번들의 이름을 하고 있지 않습니다.

암호화되어 변형된 이름이긴 하나, url과 버전이 결합된 이름으로 되어 있죠.

예를 들어 WWW.LoadFromCacheOrDownload(url, version)의 인자값에 www.url.com과 0을 넣는다고 해봅시다.

그러면 에셋 번들은 www.url.com0.unity3d와 같은 이름으로 저장된다고 보시면 됩니다. 암호화되어 바뀌지만요.

이렇게 저장하지 않으면, 서버에 존재하는 에셋 번들이 무엇인지 모르기 때문에 Cache 폴더에 캐싱되었는지

확인하려면 url에 들어가 다운로드를 받아야 하겠지요. 하지만 저장할 때 받은 url 정보를 알고 있다면 그럴 필요가 없습니다.

이제 뒤에 있는 버전 정보를 볼텐데요. 버전 정보는 에셋 번들을 캐싱할 때 결정된다고 말씀드렸죠?

두번째 인자값인 버전 정보에 int인 0을 넣었다고 가정해봅시다. 그러면 그 에셋번들은 버전 0이 됩니다.

만약 1을 넣었다면 버전 1이 될거구요 2를 넣으면 버전 2가 되는 것입니다.

```
for(int i = 0; i < 5; i++)
{
    using(WWW www = WWW.LoadFromCacheOrDownload(url, i))
    {
        yield return www;
    }
}
```

즉 이런 코드가 있다면 똑같은 에셋 번들이 버전 0부터 4까지 5개 캐싱된다고 할 수 있겠습니다.

### Caching 클래스의 제어와 Cache 폴더

제가 에셋 번들의 다운로드, 로드 부분의 스크립트에 Caching.ready 라는 코드를 사용했었습니다.

캐싱 클래스의 캐싱 작업 준비여부를 담고 있는 프로퍼티지요.

이렇듯 저희와 같이 캐싱의 형태로 저장해놓고 에셋번들을 사용하는 경우엔 Caching클래스에 대하여 알아야 합니다.

Caching 클래스의 알아두면 좋은 함수와 개념 몇가지를 짚고 넘어가려고 해요.

일단 첫번째로는 Caching.CleanCache() 입니다.

대충 늬앙스로만 봐도 캐싱 폴더를 싹 다 날려버리는 무서운 코드처럼 보이죠?

매개변수 없이 캐싱 폴더를 통째로 털어 버립니다.

구 버전에선 Caching.CleanNameCache(url)이라는 함수가 있었으나 deprecate 당했습니다.

이 함수를 써보진 않았지만, 차라리 있었으면 어떨까 싶네요.

다음은 Caching.IsVersionCached(url) 입니다.

실질적으로 에셋번들 매니져보단 패치매니저에 많이 쓰일 함수입니다. 이 url(에셋번들)의 현재 버전의 존재 여부를 리턴합니다.

다음은 Caching.maximumAvailableDiskSpace 입니다. ()가 없는 프로퍼티입니다.

이녀석은 캐시폴더가 할당할 저장장치의 용량을 설정할 수 있습니다.

byte 단위이기 때문에 1G를 설정하려면 1 * 1024 * 1024 * 1024를 하셔야 합니다. 킬로->메가->기가->테라

마지막으로 Caching.SpaceFree 입니다. 역시 () 가 없는 프로퍼티입니다.

이녀석은 읽기전용 형태로서 캐시폴더의 남은 용량을 알 수 있습니다.

두 프로퍼티 역시 패치 매니저에서 많이 사용할 것 같네요.

### 에셋 번들의 버전별 캐싱, 용량 관리법

아까 말씀드렸던 것 중에 반복문을 돌려가며 버전 정보를 다르게 하면 같은 에셋 번들이 몇개든 캐싱된다고 언급했습니다.

이러한 버전 기법이 간단하고 유용하지만, 반대로 엄청 위험한 기능일 수 있습니다.

예를 들어 50명의 캐릭터 모델링과 텍스쳐 메터리얼이 들어있는 ab_char 이라는 에셋 번들이 있다고 하겠습니다.

이는 캐시 폴더에 0번 버전으로 캐싱되어 있습니다. 이름은 ab_char0.unity3d 라고 해보죠.

그런데 디자이너분이 한 모델링의 텍스쳐를 수정하였습니다. 그렇다면 업데이트가 필요하게 되버립니다.

수정한 에셋 번들 ab_char를 버전 1로 다시 캐싱하도록 하겠습니다.

여기가 문제입니다. 다시 ab_char를 버전1로 캐싱해도 이전의 버전 0은 날라가지 않습니다.

거의 사용하는 리소스와 같은 용량의 데이터가 의미없이 존재하는 것입니다.

그렇다고 Caching.CleanCache()를 실행할 수도 없습니다. 매번 패치때마다 모든 번들을 받게 할 수는 없으니까요 ㅠㅠ

그렇기에 저희는 Caching.maximumAvailableDiskSpace와 Caching.SpaceFree를 이용한 장난을 좀 쳐야 합니다.

에셋 번들은 캐싱 폴더에 적재된 이후에 2가지 이유로 지워집니다.

첫번째는 위에서 말한 수동 삭제 Caching.CleanCache() 함수.. 저장장치를 싹다 날리는 무서운 함수죠.

두번째는 캐싱 폴더의 용량이 가득 찬 경우 입니다. 번들을 받아야 하는데 더는 저장할 공간이 없는 경우이죠. 이런 경우에 unity3d 엔진은 오래된 에셋번들부터 목을 쳐 나갑니다

한마디로 구버전의 에셋번들을 알아서 지워준다는 것이지요. 때문에 현재 클라이언트의 용량을 파악하여 적당한 최대용량을 설정하는 것이 효율적이라고 할 수 있겠습니다. 

하지만 여기에도 주의할 사항이 있습니다.

자주 업데이트되지 않는 에셋번들의 경우인데요. 예를 들면 사운드 파일이 묶인 번들같은 경우가 해당할 것입니다. 이런 자주 업데이트되지 않는 에셋번들은 자칫 삭제대상이 될 수 있으므로

가끔가다 재갱신을 해줘야 합니다. 다른 방법으로는 다운받은 에셋 번들을 Filestream등을 이용하여 다시 원하는 경로로 빼는 방법도 있긴 한데.. 전 별로 추천하고 싶지 않네요 ㅎㅎ 어려워서

Caching.CleanNameCache()를 제공하는것이 어떨까 싶네요..ㅠㅠ 무조건 편의라는 모토의 unity3d라도 이런 기능들은 개발자 재량에 조금더 맡겨줬으면 좋겠습니다.

## 에셋번들 매니저에서 도전해보자! part1. 설계편 ##

드디어 에셋번들 매니저입니다. 이 다음이 Binarydata와 Script가 될것 같군요.

일단 설계를 해놓고 코딩을 하려고 합니다. 처음 공부하는 에셋 번들이고 프로그래밍 실력도 아직 신입 수준이기에 깔금하게 나오리란 생각은 않습니다만.

중요한 부분 몇 군데 만큼은 원할하게 작동할 수 있도록 노력해보겠습니다.

### 목표 ###

로딩 상황에서 적어도 한 에셋번들의 로딩과 언로딩이 반복되서 생기는 비용 낭비를 최소화시키자.

대락적인 진행률(Progress) 상황을 알 수 있게끔 하자.

### 전재 ###

에셋 번들의 다운로드는 패치과정에서 이뤄지는 것이므로 에셋 번들 매니저에서의 WWW.LoadFromCacheOrDownload(url, version)에서는 다운로드가 일어나지 않는다.

### 설계 ###

일단 매니저를 통해 불러온 에셋 번들은 컨테이너에 적재시킨다.

각 번들별로 마지막 데이터 호출 이후 타이머를 지정해 일정 시간내에 사용 안하면 Unload 시킨다.

함수 분리를 통해 적재를 거치지 않고 단발성으로도 정보를 추출할 수 있게 한다.(게임씬 전용)

### 주의점 ###

왠만한 경우가 아니라면 에셋 번들은 항시 최종 버전만 사용하겠지만, 만일에 대비하여 버전에 대응할 수 있게 한다

어느정도 에셋번들 용량 분산을 통해서 가볍게 하겠지만 무거운 에셋 번들이 다수 로딩되어 적재될 경우 로딩 중 메모리가 과하게 사용될 수 있다. 이부분에 대한 파해법은 없을까..

### 사전실험 ###

Scripts/Manager 클래스 참조

3개의 ManagerTest에서 Manager Hi 실행

### 실험결과 ###

실행 결과
* 0
* 0 before 0.8116479
* 1
* 1 before 0.8167163
* 2
* 2 before 0.8210772
* 0 after 1.82189
* 1 after 1.825566
* 2 after 1.827567

대략 예상한 결과를 얻어냈다.

1. 한 클래스 인스턴스의 함수를 외부에서 실행시킬 때 객체 지향의 언어라도 결국 순차적으로 들어온다.
2. 하지만 순차적으로 들어가기만 할 뿐, 먼저 실행된 함수의 끝(})이나 코루틴의 딜레이시간을 기다렸다 들어오지는 않는다.
3. 원래는 이용될 에셋 번들을 미리 등록하는 리스트를 만들려 했으나, 모든 외부참조가 다른 코루틴을 가지니 필요 없을듯 하다.