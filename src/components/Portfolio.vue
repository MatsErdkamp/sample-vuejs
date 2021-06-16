<template>
  <div class="portfolio">
    <transition name="fade">
      <div v-if="loading == true" class="portfolio-loading">Loading...</div>
    </transition>

    <div class="portfolio-navigation">
      <div :key="tab['title']" v-for="(tab, index) in objects">
        <div
          class="navigation-tab-child"
          :class="{ active: index == currentSectionIndex }"
          @click="moveToSection(index)"
        >
          <div v-if="index == 0">Home</div>
          <div v-else :class="{ spacing: index == 3 || index == 8 }">
            {{ tab["title"] }}
          </div>
        </div>
      </div>
    </div>
    <canvas id="bg"></canvas>
    <div class="portfolio-text-container">
      <div></div>

      <div class="portfolio-right">
        <transition name="fade" mode="out-in" appear>
          <text-section
            v-if="sectionTransition == false"
            :key="currentSectionIndex"
            :object="objects[currentSectionIndex]"
            :index="currentSectionIndex"
            @clicked="moveToSection"
            @detailOpen="detailViewHandler"
          />
        </transition>
      </div>
    </div>
  </div>
</template>




<script>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
import { OBJLoader } from "three/examples/jsm/loaders/OBJLoader.js";
import { MTLLoader } from "three/examples/jsm/loaders/MTLLoader.js";
var hexToHsl = require("hex-to-hsl");
var hslToHex = require("hsl-to-hex");

import textSection from "./portfolio-text.vue";

const _objects = [];
const objectOne = {
  title: "Mats Erdkamp",
  subtitle: "Industrial Design Portfolio",
  read_more: false
};
const objectTwo = {
  title: "Identity",
  read_more: false,
  text:
    "Whenever new technologies emerge, I instantly become excited. From artificial intelligence to blockchains, I am always eager to learn about the inner workings of such complex technologies. Breakthrough technologies often create a plethora of new hidden opportunities. This is exciting to me as a designer, since it is up to us to uncover these hidden opportunities. This love for future technologies has had major influence on my curriculum. The DIGSIM squad, Smart and the City, Transformative Practices and USE: Robots to name a few, are all very much linked to me as a designer, and have helped me cement my vision of the future, and the role I want to have in it. A good designer does not only find hypothetical applications for new technologies however. They should also then be able to apply that knowledge and manifest it into a product. Teammates have often praised me for my technology and realization skillset, as evidenced by me often being delegated to ‘CTO’ within projects.  \
    This technology focus is a double-edged sword for me, since it can make me lose track of what is ultimately most important: the user (and society). Design is shaped by the users, and it is therefore crucial that their needs are considered, not only at the end, but throughout the entire design process. This is not always easy for me. I can get lost in cutting-edge part of a design, without considering if the user ultimately benefits. As someone who wants to bridge the gap between emerging technologies and new markets, I strive to become better at involving users during the product design. One of the benefits of designing in emerging markets, is that a good idea often immediately becomes a good business idea. I have always had an entrepreneurial spirit within me, and I hope to one day start my own company. To do this entrepreneurial spirit justice, I aim to better analyze the business side of future projects."
};
const objectThree = {
  title: "Vision",
  read_more: false,
  text:
    "I believe in a future where it is easier to interact with big data. Data-driven companies like Facebook and Google have created seemingly endless amounts of data. Most of this data however is obscured to the user. Somewhere in the back of a gigantic data center lies a solid state drive that might know you better than you know yourself. This data is highly valuable, yet we don’t give users a way to interface with it. I envision a future where people can easily access data, and have the best hardware to help them interface with said data. This ‘human-data interaction’ is something quite new. Big software companies have created the data, but hardware companies are not yet using it to its full potential. We have backends that can categorize music by emotion for example, but no hardware where one can input their current emotional state. Developing this future will create a new way of interaction. An indirect way to control digital systems. One where circumstances can lead to a better, more transparent, way of interacting with data."
};

const objectFour = {
  title: "AUD_I/O",
  read_more: true,
  subtitle: "Final Bachelor Project",
  text:
    "Car music control systems have traditionally been designed for the radio. Music streaming services often feel like an afterthought in car interfaces. The AUD_I/O project redesigns music interactions from the ground up with streaming, big data, and future mobility in mind. This fundamentally different design basis resulted in new and innovative interactions. Music recommendations are augmented by journey data, such as location, speed, road type, E.T.A., and many more. Furthermore, passengers can easiliy couple to the car's music listening session, creating a more social environment where everybody can enjoy the music. This social aspect is not limited to within the car. The AUD_I/O system even provides the opportunity to send music between cars, while waiting for the traffic light to turn green, for example."
};

const objectFive = {
  title: "Transmission Ticker",
  read_more: true,
  subtitle: "Design Research Project",
  text:
    "The rise of the Internet, IoT and Smart Cities has been resulting in an exponential global rise in data transmission and its associated environmental impact. Society currently regards the transmission of data as an inexhaustible and incomprehensible process, from which follows a difficulty of grasping data’s ecological downsides. The Transmission Ticker project explored what new perspectives on data are developed through the physicalization of data. An (auto)ethnographic study was conducted in which the Transmission Ticker, a Material Speculation which physicalizes real time household data usage, was deployed in households. The aesthetic qualities, mechanical movements and the occurrence of anthropomorphization of the artefact, triggered reflection and curiosity about data transmission in participants. Over the study, data awareness had increased, with little change in data consumption behavior. Habituation also occurred through a change in reaction to the physical and auditory stimuli. Lastly, participants had developed diverse and refined perspectives on data."
};

const objectSix = {
  title: "B.Y.C.L.M.C",
  read_more: true,
  subtitle: "Design Project",
  text:
    "I believe in a future where it is easier to interact with big data. Data-driven companies like Facebook and Google have created seemingly endless amounts of data. Most of this data however is obscured to the user. Somewhere in the back of a gigantic data center lies a solid state drive that might know you better than you know yourself. This data is highly valuable, yet we don’t give users a way to interface with it. I envision a future where people can easily access data, and have the best hardware to help them interface with said data. This ‘human-data interaction’ is something quite new. Big software companies have created the data, but hardware companies are not yet using it to its full potential. We have backends that can categorize music by emotion for example, but no hardware where one can input their current emotional state. Developing this future will create a new way of interaction. An indirect way to control digital systems. One where circumstances can lead to a better, more transparent, way of interacting with data."
};

const objectSeven = {
  title: "Breakman",
  read_more: false,
  subtitle: "Sensors for Physiology",
  text:
    "I believe in a future where it is easier to interact with big data. Data-driven companies like Facebook and Google have created seemingly endless amounts of data. Most of this data however is obscured to the user. Somewhere in the back of a gigantic data center lies a solid state drive that might know you better than you know yourself. This data is highly valuable, yet we don’t give users a way to interface with it. I envision a future where people can easily access data, and have the best hardware to help them interface with said data. This ‘human-data interaction’ is something quite new. Big software companies have created the data, but hardware companies are not yet using it to its full potential. We have backends that can categorize music by emotion for example, but no hardware where one can input their current emotional state. Developing this future will create a new way of interaction. An indirect way to control digital systems. One where circumstances can lead to a better, more transparent, way of interacting with data."
};

const objectEight = {
  title: "Other Projects",
  text:
    "After graduating from high school, I did not only apply for Industrial Design. I also applied for Computer Science at the TU/e. When I tell people about this, they aren’t surprised in the slightest. Design projects closely related to Computer Science have been prevalent throughout all three years of my bachelor’s curriculum. Looking back, I am glad that I ultimately chose for Industrial Design. The creative freedom and self-directed learning possibilities within this study have been a great fit for me, and have played a huge role in my personal development.",
  text_2:
    "Funnily enough, I am once again facing the same dilemma I faced when I was fresh out of high school. Once again am I torn between Industrial Design and Computer Science (more specifically, Artificial Intelligence). I have always tried to utilize the best of both worlds in my projects, and my future vision very much combines design with A.I. systems. The world of Industrial Design seems to be waking up to artificial intelligence however, so mastering in Industrial Design with a focus on Artificial Intelligence might be the best fit for me, since that would still give me the creative freedom I valued so much during this bachelor. I am looking forward to the next steps in my design career, and I am excited to see how the applications of A.I in design are going to evolve in the future."
};

const objectNine = {
  title: "Personal Development",
  text:
    "After graduating from high school, I did not only apply for Industrial Design. I also applied for Computer Science at the TU/e. When I tell people about this, they aren’t surprised in the slightest. Design projects closely related to Computer Science have been prevalent throughout all three years of my bachelor’s curriculum. Looking back, I am glad that I ultimately chose for Industrial Design. The creative freedom and self-directed learning possibilities within this study have been a great fit for me, and have played a huge role in my personal development.",
  text_2:
    "Funnily enough, I am once again facing the same dilemma I faced when I was fresh out of high school. Once again am I torn between Industrial Design and Computer Science (more specifically, Artificial Intelligence). I have always tried to utilize the best of both worlds in my projects, and my future vision very much combines design with A.I. systems. The world of Industrial Design seems to be waking up to artificial intelligence however, so mastering in Industrial Design with a focus on Artificial Intelligence might be the best fit for me, since that would still give me the creative freedom I valued so much during this bachelor. I am looking forward to the next steps in my design career, and I am excited to see how the applications of A.I in design are going to evolve in the future."
};

const objectTen = {
  title: "Future",
  text:
    "After graduating from high school, I did not only apply for Industrial Design. I also applied for Computer Science at the TU/e. When I tell people about this, they aren’t surprised in the slightest. Design projects closely related to Computer Science have been prevalent throughout all three years of my bachelor’s curriculum. Looking back, I am glad that I ultimately chose for Industrial Design. The creative freedom and self-directed learning possibilities within this study have been a great fit for me, and have played a huge role in my personal development.",
  text_2:
    "Funnily enough, I am once again facing the same dilemma I faced when I was fresh out of high school. Once again am I torn between Industrial Design and Computer Science (more specifically, Artificial Intelligence). I have always tried to utilize the best of both worlds in my projects, and my future vision very much combines design with A.I. systems. The world of Industrial Design seems to be waking up to artificial intelligence however, so mastering in Industrial Design with a focus on Artificial Intelligence might be the best fit for me, since that would still give me the creative freedom I valued so much during this bachelor. I am looking forward to the next steps in my design career, and I am excited to see how the applications of A.I in design are going to evolve in the future."
};

_objects.push(objectOne);
_objects.push(objectTwo);
_objects.push(objectThree);
_objects.push(objectFour);
_objects.push(objectFive);
_objects.push(objectSix);
_objects.push(objectSeven);
_objects.push(objectEight);
_objects.push(objectNine);
_objects.push(objectTen);

export default {
  name: "portfolio",
  components: {
    textSection
  },
  data: function() {
    return {
      objects: _objects,
      loading: true,
      currentSectionIndex: 0,
      sectionDelta: 1,
      sectionTransition: false,
      sectionLocationPrevious: null,
      scene: null,
      camera: null,
      destinationCameraPos: 0,
      controls: null,
      planeWidthAtDistance: null,
      planeColor: null,
      newPlaneColor: null,
      imgUrl: null,
      materialPlane: null,
      detailViewOpen: false
    };
  },
  methods: {
    detailViewHandler: function(val) {
      this.detailViewOpen = val;
      console.log(this.detailViewOpen);
    },
    moveToSection: function(index) {
      this.sectionTransition = true;
      this.sectionLocationPrevious = this.camera.position.x;
      this.sectionDelta = Math.abs(this.currentSectionIndex - index);
      var hue = 220 + index * 15;
      var hueOld = 220 + this.currentSectionIndex * 15;
      console.log(hue, hueOld);
      this.planeColor = new THREE.Color("hsl(" + hueOld + ", 20%, 20%)");
      var newColorPlane = new THREE.Color("hsl(" + hue + ", 20%, 20%)");
      this.newPlaneColor = newColorPlane;
      this.currentSectionIndex = index;
      this.destinationCameraPos = index * this.planeWidthAtDistance;
      console.log("moving camera to " + this.destinationCameraPos);
    },
    lerp: function(amt, x, y, plane) {
      var adjusted_amt = amt / Math.sqrt(this.sectionDelta);
      var start = this.camera.position.x;
      var end = this.destinationCameraPos;
      var newPos = (1 - adjusted_amt) * start + adjusted_amt * end;

      if (Math.abs(start - end) > 2) {
        this.camera.position.x = newPos;
      } else {
        if (this.sectionTransition == true) {
          this.sectionTransition = false;
          this.planeColor = this.newPlaneColor;
        }
      }

      this.controls.target.x = newPos + x;
      this.controls.target.y = -y;

      if (this.sectionTransition == true) {
        var mixAmount = Math.abs(
          (this.camera.position.x - this.sectionLocationPrevious) /
            (this.sectionLocationPrevious - this.destinationCameraPos)
        );

        plane.material.color.lerpHSL(this.newPlaneColor, mixAmount);
      }
    }
  },

  mounted() {
    var scene = new THREE.Scene();
    var camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );

    this.scene = scene;
    this.camera = camera;

    const renderer = new THREE.WebGLRenderer({
      canvas: document.querySelector("#bg")
    });

    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.shadowMap.enabled = true;
    renderer.shadowMap.type = THREE.PCFSoftShadowMap;

    camera.position.setZ(30);

    var cameraZ = camera.position.z;
    var planeZ = 0;
    var distance = cameraZ - planeZ;
    var aspect = window.innerWidth / window.innerHeight;
    var vFov = (camera.fov * Math.PI) / 180;
    var planeHeightAtDistance = 2.0 * Math.tan(vFov / 2) * distance;
    var planeWidthAtDistance = planeHeightAtDistance * aspect;
    this.planeWidthAtDistance = planeHeightAtDistance * aspect;

    var mtlLoader = new MTLLoader();
    var objLoader = new OBJLoader();

    let self = this;

    let frameObject = null;

    var url = "./frame/frame.mtl";
    mtlLoader.load(url, function(materials) {
      materials.preload();

      objLoader.setMaterials(materials);

      objLoader.load(
        "./frame/frame.obj",
        function(object) {
          object.scale.set(0.4, 0.48, 0.4);
          object.position.x = -planeWidthAtDistance / 4 + 2;
          object.position.y = -20;
          object.position.z = 0.5;

          object.traverse(function(child) {
            child.castShadow = true;
          });
          frameObject = object;
          scene.add(frameObject);
          self.loading = false;

          var newFrame = frameObject.clone();
          newFrame.position.x =
            planeWidthAtDistance - planeWidthAtDistance / 4 + 2;
          scene.add(newFrame);
          // var newFrame2 = frameObject.clone();
          // newFrame2.position.x =
          //   planeWidthAtDistance * 2 - planeWidthAtDistance / 4 + 2;
          // scene.add(newFrame2);
          var newFrame3 = frameObject.clone();
          newFrame3.position.x =
            planeWidthAtDistance * 3 - planeWidthAtDistance / 4 + 2;
          scene.add(newFrame3);
          var newFrame4 = frameObject.clone();
          newFrame4.position.x =
            planeWidthAtDistance * 4 - planeWidthAtDistance / 4 + 2;
          scene.add(newFrame4);
          var newFrame5 = frameObject.clone();
          newFrame5.position.x =
            planeWidthAtDistance * 5 - planeWidthAtDistance / 4 + 2;
          scene.add(newFrame5);
        },
        function(xhr) {},
        function(error) {
          console.log(error);
        }
      );
    });

    //SPEAKER
    const speakergeometry = new THREE.BoxGeometry(10, 30, 10);
    const speakermaterial = new THREE.MeshPhongMaterial({ color: 0xd6d6d6 });
    const speakermaterial2 = new THREE.MeshPhongMaterial({
      color: 0x875632,
      reflectivity: 0.1,
      shininess: 16
    });

    const speakerPillar = new THREE.Mesh(speakergeometry, speakermaterial);
    speakerPillar.position.x =
      planeWidthAtDistance * 2 - planeWidthAtDistance / 4 + 10;
    speakerPillar.position.y = -20;
    speakerPillar.position.z = 10;
    scene.add(speakerPillar);

    var speakerLoader = new OBJLoader();

    speakerLoader.load(
      "./speaker/speaker3.obj",
      function(object) {
        object.scale.set(10, 10, 10);
        object.rotation.set(0, 5, 0);
        object.position.x =
          planeWidthAtDistance * 2 - planeWidthAtDistance / 4 + 10;
        object.position.y = -14;
        object.position.z = 10;

        object.traverse(function(child) {
          child.castShadow = true;
          child.receiveShadow = true;
          child.material = speakermaterial2;
        });

        scene.add(object);
      },
      function(xhr) {},
      function(error) {
        console.log(error);
      }
    );

    var texture, material, picturePlane;

    texture = new THREE.TextureLoader().load("./frame/frame_picture.jpg");

    material = new THREE.MeshPhongMaterial({
      map: texture,
      reflectivity: 0.1,
      shininess: 50
    });
    picturePlane = new THREE.Mesh(new THREE.PlaneGeometry(25, 25), material);
    picturePlane.material.side = THREE.DoubleSide;
    picturePlane.position.x = -planeWidthAtDistance / 4 + 1;
    picturePlane.position.y = 3;
    picturePlane.position.z = 0.6;

    scene.add(picturePlane);

    //audio
    var textureAudio, materialAudio, pictureAudio;

    textureAudio = new THREE.TextureLoader().load("./audio.jpg");

    materialAudio = new THREE.MeshPhongMaterial({
      map: textureAudio,
      reflectivity: 0.1,
      shininess: 50
    });
    pictureAudio = new THREE.Mesh(
      new THREE.PlaneGeometry(25, 25),
      materialAudio
    );
    pictureAudio.material.side = THREE.DoubleSide;
    pictureAudio.position.x =
      planeWidthAtDistance * 3 - planeWidthAtDistance / 4 + 1;
    pictureAudio.position.y = 3;
    pictureAudio.position.z = 0.6;

    scene.add(pictureAudio);

    //BYCLMC
    var textureByclmc, materialByclmc, pictureByclmc;

    textureByclmc = new THREE.TextureLoader().load("./byclmc.jpg");

    materialByclmc = new THREE.MeshPhongMaterial({
      map: textureByclmc,
      reflectivity: 0.1,
      shininess: 50
    });
    pictureByclmc = new THREE.Mesh(
      new THREE.PlaneGeometry(25, 25),
      materialByclmc
    );
    pictureByclmc.material.side = THREE.DoubleSide;
    pictureByclmc.position.x =
      planeWidthAtDistance * 5 - planeWidthAtDistance / 4 + 1;
    pictureByclmc.position.y = 3;
    pictureByclmc.position.z = 0.6;

    scene.add(pictureByclmc);

    // ALBUM PROFESSIONAL IDENTITY
    var texture2, material2, picturePlane2;

    fetch(new Request("https://matserdkamp.com/api/recent/?limit=1&users=2"))
      .then(response => {
        return response.json();
      })
      .then(data => {
        var imgUrl = data[0]["song"]["album"]["image"]["px640"];

        var texture2Loader = new THREE.TextureLoader();
        texture2Loader.crossOrigin = "Anonymous";
        texture2 = texture2Loader.load(imgUrl);

        material2 = new THREE.MeshPhongMaterial({
          map: texture2,
          reflectivity: 0.1,
          shininess: 50
        });
        picturePlane2 = new THREE.Mesh(
          new THREE.PlaneGeometry(25, 25),
          material2
        );
        picturePlane2.material.side = THREE.DoubleSide;
        picturePlane2.position.x =
          planeWidthAtDistance - planeWidthAtDistance / 4 + 1;
        picturePlane2.position.y = 3;
        picturePlane2.position.z = 0.6;

        scene.add(picturePlane2);
      });

    //Create a PointLight and turn on shadows for the light
    const light = new THREE.PointLight(0xffffff, 1, 100);
    light.position.set(-planeWidthAtDistance / 4, 15, 30);
    light.castShadow = true; // default false

    //Set up shadow properties for the light
    light.shadow.mapSize.width = 1024; // default
    light.shadow.mapSize.height = 1024; // default
    scene.add(light);

    var light2 = light.clone();
    light2.position.x = planeWidthAtDistance - planeWidthAtDistance / 4;
    scene.add(light2);

    var light3 = light.clone();
    light3.position.x = planeWidthAtDistance * 2 - planeWidthAtDistance / 4;
    scene.add(light3);

    var light4 = light.clone();
    light4.position.x = planeWidthAtDistance * 3 - planeWidthAtDistance / 4;
    scene.add(light4);

    var light5 = light.clone();
    light5.position.x = planeWidthAtDistance * 4 - planeWidthAtDistance / 4;
    scene.add(light5);

    var light6 = light.clone();
    light6.position.x = planeWidthAtDistance * 5 - planeWidthAtDistance / 4;
    scene.add(light6);

    var light7 = light.clone();
    light7.position.x = planeWidthAtDistance * 6 - planeWidthAtDistance / 4;
    scene.add(light7);

    const lightAmbient = new THREE.AmbientLight(0xffffff, 0.5); // soft white light
    scene.add(lightAmbient);

    const controls = new OrbitControls(camera, renderer.domElement);
    controls.enabled = false;
    this.controls = controls;

    // const texturePlane = new THREE.TextureLoader().load("./stucco.jpg");
    const texturePlaneNormal = new THREE.TextureLoader().load(
      "./stuccoNormal.jpg"
    );

    this.planeColor = new THREE.Color("hsl(" + 220 + ", 20%, 20%)");

    texturePlaneNormal.wrapS = THREE.RepeatWrapping;
    texturePlaneNormal.wrapT = THREE.RepeatWrapping;
    texturePlaneNormal.repeat.x = planeWidthAtDistance * 2;
    texturePlaneNormal.repeat.y = planeHeightAtDistance / 2.5;
    const materialPlane = new THREE.MeshPhongMaterial({
      color: this.planeColor,
      normalMap: texturePlaneNormal,
      normalScale: new THREE.Vector2(10, 10),
      reflectivity: 0.1,
      shininess: 10
    });

    this.materialPlane = materialPlane;

    const geometryPlane = new THREE.PlaneGeometry(
      planeWidthAtDistance * 20,
      planeHeightAtDistance * 1.2
    );

    const plane = new THREE.Mesh(geometryPlane, this.materialPlane);
    plane.receiveShadow = true;
    scene.add(plane);

    let x = 0;
    let y = 0;

    document.addEventListener("mousemove", e => {
      x = e.clientX / window.innerWidth - 0.5;
      y = e.clientY / window.innerHeight - 0.5;
    });

    function animate() {
      requestAnimationFrame(animate);

      if (self.detailViewOpen == false) {
        renderer.render(scene, camera);
        self.lerp(0.1, x * 2.5, y * 2.5, plane);
        controls.update();
      }
    }

    animate();
  }
};
</script>

<style>
#bg {
  position: absolute;
  top: 0;
  left: 0;
  max-width: 100%;
  max-height: 100%;
}

.portfolio {
  overflow: hidden;
}

.portfolio-loading {
  position: absolute;
  top: 0;
  width: 100%;
  height: 100%;
  background: black;
  color: white;
  display: flex;
  z-index: 10000;
  font-size: 3em;
  font-weight: 600;
  align-items: center;
  justify-content: center;
}

.portfolio-text-container {
  position: absolute;
  right: 0;
  top: 0;
  grid-template-columns: 50% 50%;
  width: 50%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.portfolio-right {
  text-align: start;
  color: white;
  padding: 0px 2vw;
  min-height: 100%;
  width: 100%;
  display: flex;
  align-items: center;
}

.text-title {
  text-shadow: 4px 4px 4px rgba(0, 0, 0, 0.6);
  font-size: 4em;
  font-weight: 600;
  line-height: 1;
  margin-bottom: 12px;
  margin-top: -24px;
}

.text-subtitle {
  text-shadow: 4px 4px 4px rgba(32, 20, 20, 0.6);
  font-size: 1.6em;
  font-weight: 400;
  margin-bottom: 24px;
  margin-top: -12px;
}

.text-body {
  text-align: justify;
  font-size: 1em;
  line-height: 1.1;
  font-weight: 400;
  color: white !important;
}

.text-button {
  display: inline-block;
  background: transparent;
  border: 1px solid white;
  color: white;
  font-size: 1.4em;
  border-radius: 8px;
  width: max-content;
  padding: 12px;
  margin-top: 24px;
  transition: all 200ms ease;
  opacity: 1;
  cursor: pointer;
}

.text-button-more {
  display: inline-block;
  border: 1px solid white;
  color: white;
  font-size: 1.4em;
  border-radius: 8px;
  width: max-content;
  padding: 12px;
  margin-top: 24px;
  transition: all 200ms ease;
  opacity: 1;
  cursor: pointer;
  background: rgba(255, 255, 255, 0.2);
}

.text-button-more:hover {
  background: rgba(255, 255, 255, 0.3);
}

.text-button:hover {
  background: rgba(255, 255, 255, 0.1);
}

.portfolio-navigation {
  position: fixed;
  width: 100%;
  bottom: 0;
  background: rgba(0, 0, 0, 0.6);
  height: 5vh;
  z-index: 100;
  display: flex;
  width: 100%;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}

.navigation-tab-child {
  height: 100%;
  padding: 0 12px;
  color: grey;
  cursor: pointer;
  transition: 100ms ease all;
}

.active {
  color: white;
}

.spacing {
  padding-left: 24px;
  border-left: 1px solid rgb(56, 56, 56);
}

.navigation-tab-child:hover {
  color: lightgray;
}

.fade-enter-active {
  transition: all 500ms ease;
}

.fade-leave-active,
.fade-move {
  transition: all 500ms ease;
}

.fade-leave-to,
.fade-enter-from {
  opacity: 0;
}

.fade-leave-from,
.fade-enter-to {
  opacity: 1;
}
</style>