#

1. CNN에서 생성된 representation을 cluster하여 patch당 deep feature representation을 뽑아낸다. 
2. Cluster label를 supervisory signal로 이용하여 CNN parameter를 업데이트한다.
3. CNN에서 뽑힌 deep representation에 k-means를 적용한다. 
4. 타겟 이미지에 cluster label을 project해서 segmented image를 얻는다.


