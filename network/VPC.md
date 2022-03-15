# VPN과 VPC

![](https://images.velog.io/images/jun-k0/post/befa678a-8025-46d6-a0de-8a6479501fba/IMG_2DF10E2BC06B-1.jpeg)

- VPN(Virtual Private Network) : 실제 사설망이 아닌 가상 사설망
- VPC(Virtual Private Cloud) : EC2 인스턴스들을 나눠줌. VPC별로 네트워크를 구성할 수 있고, 각각의 VPC는 독립된 네트워크처럼 작동한다.

VPC를 구축하기 위해선 VPC의 아이피 범위를 사설 아이피 대역에 맞추어 구축해야함. 

## 사설아이피
인터넷을 위해 사용하는것이 아닌 우리끼리 사용하는 아이피주소 대역.

- “안방”, “작은방”, “큰방”처럼 내부에서 쓰는 주소를 사설아이피 대역이라고 부르며 내부 네트워크 내에서 위치를 찾아갈때 사용한다.

- 내 친구가 찾아올때는 도로명주소(퍼블릭 아이피)를 알려주면 되고, 같은집(우리집)에 사는(동일한 네트워크 상 존재하는) 내가 동생한테 갈때는 동생방(사설아이피)로 찾아가게된다.

# 서브넷, 라우터, 게이트웨이

- 서브넷 : VPC를 잘개 쪼개는 과정. 서브넷을 나누는 이유는 **더 많은 네트워크망을 만들기 위해서**
- 라우터 : 목적지
- 라우팅 테이블 : 각 목적지에 대한 이정표
- 인터넷 게이트웨이 : VPC와 인터넷을 연결해주는 관문. 우선 목적지 주소가 VPC안의 범위(172.31.0.0/16)인지 확인 후, 매칭되지 않으면 IGA A(인터넷 게이트웨이 A)로 보낸다.
- 퍼블릭 서브넷 : 인터넷과 연결되어있는 서브넷
- 프라이빗 서브넷 : 인터넷과 연결되어 있지 않은 서브넷

# 네트워크 ACL, 보안 그룹, NAT 게이트웨이

![](https://images.velog.io/images/jun-k0/post/3d67e8b8-62d5-4b07-85b3-d02d9eba5655/IMG_0392AEC30DE6-1.jpeg)

네트워크 ACL과 보안 그룹은 방화벽과 같은 역활을 하며 인바운드 트래픽과 아웃바운드 트래픽 보안 정책을 설정할 수 있다. 네트워크ACL과 보안그룹이 충돌한다면 보안그룹이 더 높은 우선순위를 갖는다.

- 네트워크 ACL : Stateless하게 작동하며 모든 트래픽이 기본설정 되어있기 때문에 불필요한 트래픽을 막도록 적용해야함. 서브넷단위로 적용되며 리소스별로는 설정할 수 없다.
**여기서, Stateless(무상태)란?** HTTP가 기본적으로 가지고 있는 특징. 비즈니스 로직 과정을 지날 수록 전달 내용이 축적되어 길어진다는 단점이 있다. 특별한 일이 없다면 무상태를 지향해야하며 정말 필요한 경우에만 Stateful(상태유지) 해야 한다. 

- 보안 그룹 : Stateful한 방식으로 동작하는 보안그룹은 모든 허용을 차단하도록 기본설정 되어있으며 필요한 설정은 허용해주어야함. 또한 네트워크ACL과 다르게 각각의 보안그룹별로도 별도의 트래픽을 설정할 수 있으며 서브넷에도 설정할 수 있지만 각각의 EC2인스턴스에도 적용할 수 있음.

- NAT 게이트웨이 : 프라이빗 서브넷이 인터넷과 통신하기위한 아웃바운드 인스턴스. 프라이빗 네트워크가 외부에서 요청되는 인바운드는 필요 없더라도 인스턴스의 펌웨어나 혹은 주기적인 업데이트가 필요하여 아웃바운드 트래픽만 허용되야할 경우가 있다. 이때 퍼블릭 서브넷상에서 동작하는 NAT 게이트웨이는 프라이빗 서브넷에서 외부로 요청하는 아웃바운드 트래픽을 받아 인터넷 게이트웨이와 연결해준다.

# Reference
https://medium.com/harrythegreat/aws-%EA%B0%80%EC%9E%A5%EC%89%BD%EA%B2%8C-vpc-%EA%B0%9C%EB%85%90%EC%9E%A1%EA%B8%B0-71eef95a7098