<template>
  <div>
    <div class="snippet-parent" v-if="snippet.conclusion == undefined">
      <img class="snippet-image" v-if="snippet.title != null" v-bind:src="findSource()" />
      <div class="snippet-text-parent">
        <div class="snippet-title">{{ snippet.title }}</div>

        <div class="snippet-text">
          <p>{{ snippet.text }}</p>
          <p v-if="snippet.text_2 != undefined">
            {{ snippet.text_2 }}
          </p>
        </div>
      </div>
    </div>
    <div class="snippet-parent-center" v-else>
      <div class="snippet-title-center">
        {{ snippet.title }}
                
      </div>
      <p class="snippet-text-center">{{ snippet.text }}</p>
      <p v-if="snippet.text_2 != undefined" class="snippet-text-center">
        {{ snippet.text_2 }}
      </p>
      <p v-if="snippet.text_3 != undefined" class="snippet-text-center">
        {{ snippet.text_3 }}
      </p>

      <div v-if="snippet.title == null" class="snippet-text-center">
        Thinking back on the bachelor as a whole, I think it is fair to say that
        I gained a lot of new skills. These past three years have not only
        shaped my capabilities, but also my worldview and personality. Once you
        are a designer, everything in the world suddenly becomes a design. In my
        opinion, this new mindset is just as important as the concrete skills
        learned at a university. An education can severely shape your worldview.
        I think that, to excel in any profession, you should never be able to
        ‘turn off’ that part of yourself. A good should architect always get
        excited when seeing a beautiful building. Similarly, designers should
        always get excited by good design. I am glad that this is the case for
        me personally, since it suggests a future where I can be genuinely
        excited with what I do, instead of having to drag myself to work every
        day. <br><br> While this ambition is crucial, the concrete skills also play a big
        role of course. Throughout the bachelor’s program, I became a better
        designer is almost every way imaginable. Some of these did not come as a
        surprise however. I almost became a Computer Scientist instead, so it is
        not strange to me that I have a well-developed skillset in the expertise
        area of Math, Data & Computing. I think that my mastery in this
        expertise area, together with Technology and Realization, needs little
        to no verbal explanation. These expertise areas have been front and
        center throughout all my projects, and I think that even this portfolio
        can communicate that quite well. It is only fitting that I concluded the
        bachelor’s program with the most difficult task in both these expertise
        areas yet. The software developed, and the physical input device, far
        exceed what I would deem minimal competence. <br><br> As mentioned before, the
        almost Computer Scientist who is good at the Computer Science related
        expertise areas will come as a surprise to no one. The real challenge
        lies in the development of the skills that a typical Computer Scientist
        would not have, and I think I have achieved that in such a way that I
        would definitely provide value to a team of actual Computer Scientist.
        My aesthetic quality has improved quite significantly over the years in
        my opinion, but in this case, aesthetics speak louder than words, so it
        is probably easiest to look at work. One area of creativity and
        aesthetics where I can improve in my opinion is being creative in the
        ideation process. I tend to ponder about a concept for a week straight,
        with a lot of it happening in my head. In a sense, this seems to work
        for me, but more creative approaches to idea generation might be
        beneficial to the project as they might speed along the process, as well
        as provide more evidence for the steps that I take. This second point is
        closely related to what I consider my own biggest shortcoming in the
        soft skills: effective communication of my process. My balance between
        doing work and reporting on that work is skewed too much towards the
        former. I think that, especially when working for a company, it becomes
        increasingly important to properly communicate activities. One area
        where this shortcoming became apparent is in the reporting of the user
        testing during the final bachelor project. I did more testing than the
        final report communicates. I will strive to in the future better reflect
        all the things I do. <br> <br>Speaking of user testing, I do think that my skills
        in this area have sufficiently developed during the bachelor. I consider
        the societal implications way more nowadays than when I started studying
        at the TU/e. Two of my projects even tried to highlight the negative
        cultural and ethical implications that certain future technologies might
        bring about in society (Transmission Ticker & B.Y.C.L.M.C.). If a
        project really excites me personally, I might lose track of the users
        desires. It can be difficult to find a balance between intuition and
        external validation. But, as far as the concrete skills of testing a
        user go, I think that I developed those quite well during the courses
        User-centered Design and Design - Research. But it is the Transmission
        Ticker project best proves the competence in this area. This research
        was close to being published (unfortunately that fell through), but that
        should give an indication of what was achieved in the research there.
        <br> <br>Then last to discuss is the development of the business and
        entrepreneurship expertise area. Since high school I have been starting
        businesses on the side, and this entrepreneurial spirit is still very
        much alive in me today. Starting right at the ideation phase, I am
        always considering potential commercial interest. Commercial viability
        plays a big role in my enjoyment of a project. Every step along the way
        I try to imagine myself selling the product. I think it would certainly be beneficial to further develop my skills in this area, and I am hoping to do that through the start of my own company <br><br> If I
        were to describe myself now, I am a product developer who lives on the
        border between Industrial Design and Computer Science. A hard worker who
        ponders about all kinds of design considerations and implications, some
        of which unfortunately never get to meet a piece of paper. I can become
        obsessive over things that interest me, sometimes to a fault. The world
        of design interests me greatly and I am glad to have concluded these
        three years with a passion even greater than I had at the start.
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["snippet"],
  methods: {
    findSource: function() {
      var string = this.snippet.image;

      return string;
    }
  }
};
</script>

<style>
.snippet-parent {
  display: flex;
  margin: 20px;
  min-width: 100%;
  align-items: center;
}

.snippet-parent-center {
  margin-top: 48px;
  min-width: 100%;
  height: max-content;
  text-align: center;
}

.snippet-text-parent {
  padding: 0 24px;
}

.snippet-title {
  font-size: 2.4em;
  font-weight: 500;
  line-height: 1.4;
  margin-bottom: 12px;
}

.snippet-title-center {
  font-size: 2.4em;
  font-weight: 500;
  line-height: 1.4;

  margin-bottom: 12px;
  text-align: center;
}

.snippet-subtitle {
  font-size: 1em;
  color: grey;
}

.snippet-text {
  font-size: 1em;
}

.snippet-text-center {
  font-size: 1em;
  display: block;
  width: 50%;
  height: max-content;
  margin: auto;
  text-align: justify;
}

.snippet-image {
  border-radius: 4px;
  width: 20vw;
  background: grey;
  height: 20vw;
}
</style>