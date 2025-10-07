* Functional Requirement
    - Upload video
    - Watch video

* Non Functional Requirement
    - Realibility(uploaded video should not get lost)
    - Availability > Consistency (Its ok if somebody just uploaded video is not reflecting everywhere, but it is necessary that everytime user refreshes users shoul get some videos)
    - low letency streaming in low bandwidth
    - Support for uploading/streaming large videos 256GB

* Scale 
- 100:1 (Users watching:users uploading)
- 100 million views, 1 millions uploads


Requirement(Functional,Non-Functional)
Core Entities
API Interface
Data flow
High Level Design
Deep dives

Core Entities
Video
Video metadata
User

API
upload video
POST /video
{
    VideoMetaData
}

//Watch video
GET /Video/:videoId

