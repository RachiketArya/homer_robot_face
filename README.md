# homer_robot_face

![Robot face](./images/robot_face.png)

This package contains a robot face as described in the paper "Enhancing Human-Robot Interaction by a Robot Face with Facial Expressions and Synchronized Lip Movements".


### Abstact

With service robots becoming increasingly elaborate for higher level tasks,  human-robot interaction is moving into the focus of robotic research.  In this paper we present an animated robot face as a convenient way of interacting with  robots.   Our  robot face  can  show  7  different facial  expression,  thus providing a  robot with  the ability to express emotions.  This capability is crucial for robots to be accepted as everyday companions in domestic environments.  Aiming towards a more realistic interaction experience our robot face moves its lips synchronously to the synthesized speech.  In a broad user study with 100 subjects we test the emotions conveyed by the robot face.   The results indicate that our robot face enhances human robot interaction by providing the robot with the ability to express emotions.  The presented robot face is highly customizable.  It is available for ROS and can be used with any robot that integrates ROS in its architecture


## Instruction

Launch the face node like:

```
roslaunch homer_robot_face robot_face.launch
```

For speech synthesis you can use the `homer_tts` package which contains a collection of
different text to speech synthesizers.


## Topics

* `/recognized_speech` Shows recognized speech in the bottom of the face to support humans in interacting with the robot.
* `/robot_face/ImageDisplay` An Image can be displayed optionally (this topic awaits a image as message).
* `/robot_face/ImageFileDisplay` Alternatively a filename can be specified.
* `/robot_face/expected_input` An additional text on the bottom of the robot face will be shown. Here you can display instructions depending on the state.
* `/robot_face/talking_finished` When coupled with a TTS system this topic will be sent once the robot finished speaking.
* `/robot_face/text_out` When coupled with a TTS system this topic will be used for synthesizing speech.


If you use this package consider citing:

```
@article{seib2013enhancing,
  title={Enhancing human-robot interaction by a robot face with facial expressions and synchronized lip movements},
  author={Seib, Victor and Giesen, Julian and Gr{\"u}ntjens, Dominik and Paulus, Dietrich},
  year={2013},
  publisher={V{\'a}clav Skala-UNION Agency}
}
```

```
@inproceedings{seib2015team,
  title={Team homer@ unikoblenz—approaches and contributions to the robocup@ home competition},
  author={Seib, Viktor and Manthe, Stephan and Memmesheimer, Raphael and Polster, Florian and Paulus, Dietrich},
  booktitle={Robot Soccer World Cup},
  pages={83--94},
  year={2015},
  organization={Springer}
}
```
