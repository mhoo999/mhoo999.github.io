---
layout: post
title: Escape From Sesac
description: 멀티플레이 익스트렉션 루트 슈팅 장르 게임 개발 프로젝트
img: assets/img/sesac_project05/thumbnail.gif
importance: 1
category: SESAC(2308 ~ 2403)
toc:
  - name: 게임 소개 영상
  - name: 플레이 영상
  - name: 프로젝트 기록
  - name: 개요
  - name: 진행 내용
  - name: 결과
---

## 게임 소개 영상

[![프로젝트 영상 보기](https://img.youtube.com/vi/uRh-SR5xiZY/0.jpg)](https://youtu.be/uRh-SR5xiZY "프로젝트 영상 - 클릭하여 시청")

## 플레이 영상

[![프로젝트 영상 보기](https://img.youtube.com/vi/5dy0EVVp6bU/0.jpg)](https://youtu.be/5dy0EVVp6bU "프로젝트 영상 - 클릭하여 시청")

---

## 프로젝트 기록

<a href="https://github.com/Sho1007/SesacProject5" target="_blank">프로젝트 깃 링크 바로가기</a><br>
<a href="https://www.notion.so/5-Escape-from-Sesac-f4880bc1f6914df0816617124138b72d?pvs=4" target="_blank">프로젝트 Notion 페이지 바로가기</a>

---

## 개요

|       Duration       |      Team Size     |      SKILL      |
| :------------------: | :----------------: | :-------------: |
| 24/02/01 ~ 24/03/08  |         2          |  UE5.3, BP, C++ |

멀티플레이 익스트랙션 루트 슈팅 장르 게임 프로젝트로 다중 인원이 특정 맵에 진입하여 타겟 제거, 아이템 획득 등의 목적을 수행하고 무사히 탈출하는 것이 목적인 게임.
멀티플레이와 사용자 조작 및 여정, AI의 역할 수행을 개발 중점으로 목표하고 진행한 프로젝트.

AI와 퀘스트, 전반적인 기획을 담당.

---

## 진행 내용

### AI FSM 설계
디자인 패턴 중 하나인 상태 패턴을 선택하여 객체지향적인 ai 설계를 기획했다. 대충 아래와 같이 간단한 다이어그램을 스케치하고 작업을 시작.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/sesac_project05/img0.jpg" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

최종적으로 구현한 상태는 Patrol, Search, Chase. AI는 기본 Patrol 상태로, 가지고 있는 waypoint array 내 좌표를 순서대로 이동하며 순찰한다. 적이 시야에 들어오거나, 총 소리에 반응하여 Search 상태로 바뀌며, 해당 위치를 조사한다. 적을 발견하면 Chase 상태로 변경되고 사격이 가능할 경우 발포한다.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/sesac_project05/img1.jpg" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

상태 패턴을 사용함으로써, 각 상태를 별도의 클래스로 분리하여 관리할 수 있었고, 상태 전환 로직을 중앙(controller)에서 제어할 수 있게 만들었다. 이는 유지보수와 기능 확장 시 큰 장점으로 작용하였다.

```c++
void AEOSAIController::SetContext(EEnemystate next)
{
	FSMInterface->StopExecute();
	
	switch (next) {
	case EEnemystate::patrol:			FSMInterface = FSMPatrolComp;			break;
	case EEnemystate::search:			FSMInterface = FSMSearchComp;			break;
	case EEnemystate::chase:			FSMInterface = FSMChaseComp;			break;
	case EEnemystate::retreatFiring:
		break;
	case EEnemystate::AdvanceFiring:
		break;
	case EEnemystate::evade:
		break;
	case EEnemystate::camping:
		break;
	case EEnemystate::selfHealing:
		break;
	}
	state = next;
}
```

각 상태에서 controller의 SetContext 함수를 호출하여 조건에 따라 상태를 변경한다.

### AI Spawn

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/sesac_project05/img3.jpg" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

AI는 AISpawner 인스턴스에서 생성되는데, 생성시 해당 인스턴스가 가지고 있는 waypointArray를 전달받는다. 그럼 전달받은 waypoint 순서대로 순찰을 시작한다.
```c++
void AAISpawnManager::MakeScave()
{
	if (HasAuthority())
	{
		GetWorld()->GetTimerManager().ClearTimer(respawnTimer);
		FVector SpawnLoc = this->GetActorLocation();
		AScavBase* SpawnActor = GetWorld()->SpawnActor<AScavBase>(ScavFactory, SpawnLoc, FRotator::ZeroRotator);

		if (SpawnActor)
		{
			UE_LOG(LogTemp, Warning, TEXT("Spawn Scav!"));
			SpawnActor->GetController<AEOSAIController>()->SetWaypoint(waypointArray);
			SpawnActor->GetComponentByClass<UHealthComponent>()->OnIsDeadChanged.AddUObject(this, &AAISpawnManager::RespawnScave);

			if (auto objectiveComp = SpawnActor->GetComponentByClass<UObjectiveComponent>())
			{
				objectiveComp->SetObjectID(objectID);
				objectiveComp->SetValue(value);
			}
		}
	}
}
```

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/sesac_project05/img2.jpg" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


AI는 사망 시 본인을 생성한 spawner에게 죽었다고 알려준다. 그럼 spawner는 정해진 시간 이후, 다시 AI를 생성한다.
```c++
void AAISpawnManager::RespawnScave(bool bNewIsDead)
{
	if (bNewIsDead)
	{
		GetWorld()->GetTimerManager().SetTimer(respawnTimer, this, &AAISpawnManager::MakeScave, respawnTime, false);
	}
}
```

### Body parts targeting
우리 게임에서는 몸통 부위별로 받는 데미지가 다르고, 중요 부위 손상이 아닌 경우 사망에 이르지 않기 때문에 `부위 타격`기능이 중요했다. 그래서 AI는 플레이어를 인지 했을 때, 보이는 부위를 스캔하고 우선 순위를 매겨 해당 부위를 타격할 수 있도록 아래와 같이 코드를 작성했다. 최종적으로 `발견 여부`와 `Targer Location`을 반환한다.
```c++
bool UFSM_Chase_Component::FocusTargetPart(AActor* targetActor, FVector& TargetLocation)
{
	if (targetActor == nullptr)
	{
		return false;
	}

	ACharacter* TargetCharacter = Cast<ACharacter>(targetActor);
	if (TargetCharacter == nullptr)
	{
		return false;
	}
		
	TArray<FName> Sockets = TargetCharacter->GetMesh()->GetAllSocketNames();

	TargetPart currentTarget = {"NONE", 4, true};
	
	for (FName Socket : Sockets)
	{
		FString SocketName = Socket.ToString();
		FVector SocketLocation = TargetCharacter->GetMesh()->GetSocketLocation(Socket);

		FHitResult checkResult;
		
		if (GetWorld()->LineTraceSingleByChannel(checkResult, ai->GetActorLocation(), SocketLocation, ECC_Visibility))
		{
			if (SocketName.Equals("spine_03", ESearchCase::IgnoreCase) && checkResult.GetActor() == targetActor)
			{
				TargetPart THORAX {"THORAX", 1, true, SocketLocation};
				if (THORAX.priority < currentTarget.priority)
				{
					currentTarget = THORAX;
				}
			}
			else if (SocketName.Equals("spine_01", ESearchCase::IgnoreCase) && checkResult.GetActor() == targetActor)
			{
				TargetPart STOMACH {"STOMACH", 2, true, SocketLocation};
				if (STOMACH.priority < currentTarget.priority)
				{
					currentTarget = STOMACH;
				}
			}
			else if (SocketName.Equals("head", ESearchCase::IgnoreCase) && checkResult.GetActor() == targetActor)
			{
				TargetPart HEAD {"HEAD", 3, true, SocketLocation};
				if (HEAD.priority < currentTarget.priority)
				{
					currentTarget = HEAD;
				}
			}
			else if (SocketName.Equals("Hand_R", ESearchCase::IgnoreCase) && checkResult.GetActor() == targetActor)
			{
				TargetPart RIGHTARM {"RIGHTARM", 3, true, SocketLocation};
				if (RIGHTARM.priority < currentTarget.priority)
				{
					currentTarget = RIGHTARM;
				}
			}
			else if (SocketName.Equals("Hand_L", ESearchCase::IgnoreCase) && checkResult.GetActor() == targetActor)
			{
				TargetPart LEFTARM {"LEFTARM", 3, true, SocketLocation};
				if (LEFTARM.priority < currentTarget.priority)
				{
					currentTarget = LEFTARM;
				}
			}
			else if (SocketName.Equals("calf_r", ESearchCase::IgnoreCase) && checkResult.GetActor() == targetActor)
			{
				TargetPart RIGHTLEG {"RIGHTLEG", 3, true, SocketLocation};
				if (RIGHTLEG.priority < currentTarget.priority)
				{
					currentTarget = RIGHTLEG;
				}
			}
			else if (SocketName.Equals("calf_l", ESearchCase::IgnoreCase) && checkResult.GetActor() == targetActor)
			{
				TargetPart LEFTLEG {"LEFTLEG", 3, true, SocketLocation};
				if (LEFTLEG.priority < currentTarget.priority)
				{
					currentTarget = LEFTLEG;
				}
			}
		}	
	}

	if (currentTarget.name == "NONE")
	{
		return false;
	}
	
	TargetLocation = currentTarget.partLoc;
	return true;
}
```

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/sesac_project05/gif0.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

보이는 특정 부위를 조준하고 발포하지만... 탄이 튀는 것을 구현했기 때문에 랜덤하게 타격받는 것을 볼 수 있다.

### UI 작업



### 퀘스트 구조 설계



---

## 결과



---
