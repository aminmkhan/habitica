<template lang="pug">
.row
  .clearfix.col-8.standard-page
    .row
      .col-6.title-details
        h1(v-once) {{ $t('welcomeToTavern') }}

    .row.chat-row
      .col-12
        h3(v-once) {{ $t('welcomeToTavern') }}

        textarea(:placeholder="$t('chatPlaceHolder')")
        button.btn.btn-secondary.send-chat.float-right(v-once) {{ $t('send') }}

        .container.community-guidelines(v-if='communityGuidelinesAccepted')
          .row
            div.col-8(v-once) {{ $t('communityGuidelinesIntro') }}
            div.col-4
              button.btn.btn-info(@click='acceptCommunityGuidelines()', v-once) {{ $t('acceptCommunityGuidelines') }}

        .hr
          .hr-middle(v-once) {{ $t('today') }}

        .row
          .col-md-2
            .svg-icon(v-html="icons.like")
          .col-md-10
            .card(v-for="msg in group.chat", :key="msg.id")
              .card-block
                h3.leader Character name
                span 2 hours ago
                .clearfix
                  strong.float-left {{msg.user}}
                  .float-right {{msg.timestamp}}
                .text {{msg.text}}
                hr
                span.action(v-once)
                  .svg-icon(v-html="icons.like")
                  | {{$t('like')}}
                span.action(v-once)
                  .svg-icon(v-html="icons.copy")
                  | {{$t('copyAsTodo')}}
                span.action(v-once)
                  .svg-icon(v-html="icons.report")
                  | {{$t('report')}}
                span.action(v-once)
                  .svg-icon(v-html="icons.delete")
                  | {{$t('delete')}}
                span.action.float-right
                  .svg-icon(v-html="icons.liked")
                  | +3

  .col-md-4.sidebar
    .section
      .grassy-meadow-backdrop

      .sleep
        strong(v-once) {{ $t('sleepDescription') }}
        ul
          li(v-once) {{ $t('sleepBullet1') }}
          li(v-once) {{ $t('sleepBullet2') }}
          li(v-once) {{ $t('sleepBullet3') }}
          li(v-once) {{ $t('sleepBullet4') }}
        button.btn.btn-secondary.pause-button(v-if='!user.preferences.sleep', @click='toggleSleep()', v-once) {{ $t('pauseDailies') }}
        button.btn.btn-secondary.pause-button(v-if='user.preferences.sleep', @click='toggleSleep()', v-once) {{ $t('unpauseDailies') }}

    .section-header
      .row
        .col-10
          h3(v-once) {{ $t('staffAndModerators') }}
        .col-2
          .toggle-up(@click="sections.staff = !sections.staff", v-if="sections.staff")
            .svg-icon(v-html="icons.upIcon")
          .toggle-down(@click="sections.staff = !sections.staff", v-if="!sections.staff")
            .svg-icon(v-html="icons.downIcon")
      .section.row(v-if="sections.staff")
        .col-3.staff(v-for='user in staff', :class='{staff: user.type === "Staff", moderator: user.type === "Moderator", bailey: user.name === "It\'s Bailey"}')
          .title {{user.name}}
          .type {{user.type}}

    .section-header
      .row
        .col-10
          h3(v-once) {{ $t('helpfulLinks') }}
        .col-2
          .toggle-up(@click="sections.staff = !sections.staff", v-if="sections.staff")
            .svg-icon(v-html="icons.upIcon")
          .toggle-down(@click="sections.staff = !sections.staff", v-if="!sections.staff")
            .svg-icon(v-html="icons.downIcon")
      .section.row(v-if="sections.staff")
        ul
          li
            a(herf='', v-once) {{ $t('communityGuidelinesLink') }}
          li
            a(herf='', v-once) {{ $t('lookingForGroup') }}
          li
            a(herf='', v-once) {{ $t('faq') }}
          li
            a(herf='', v-once) {{ $t('glossary') }}
          li
            a(herf='', v-once) {{ $t('wiki') }}
          li
            a(herf='', v-once) {{ $t('dataDisplayTool') }}
          li
            a(herf='', v-once) {{ $t('reportProblem') }}
          li
            a(herf='', v-once) {{ $t('requestFeature') }}
          li
            a(herf='', v-once) {{ $t('communityForum') }}
          li
            a(herf='', v-once) {{ $t('askQuestionGuild') }}

    .section-header
      .row
        .col-10
          h3(v-once) {{ $t('playerTiers') }}
        .col-2
          .toggle-up(@click="sections.staff = !sections.staff", v-if="sections.staff")
            .svg-icon(v-html="icons.upIcon")
          .toggle-down(@click="sections.staff = !sections.staff", v-if="!sections.staff")
            .svg-icon(v-html="icons.downIcon")
      .section.row(v-if="sections.staff")
        .col-12
          p(v-once) {{ $t('playerTiersDesc') }}
          ul.tier-list
            li.tier1(v-once) {{ $t('tier1') }}
            li.tier2(v-once) {{ $t('tier2') }}
            li.tier3(v-once) {{ $t('tier3') }}
            li.tier4(v-once) {{ $t('tier4') }}
            li.tier5(v-once) {{ $t('tier5') }}
            li.tier6(v-once) {{ $t('tier6') }}
            li.tier7(v-once) {{ $t('tier7') }}
            li.moderator(v-once) {{ $t('tierModerator') }}
            li.staff(v-once) {{ $t('tierStaff') }}
            li.npc(v-once) {{ $t('tierNPC') }}
</template>

<style lang='scss' scoped>
  @import '~client/assets/scss/colors.scss';

  // @TODO: Move chat to component
  .chat-row {
    position: relative;

    .community-guidelines {
      background-color: rgba(135, 129, 144, 0.84);
      padding: 1em;
      color: $white;
      position: absolute;
      top: 0;
      height: 150px;
      padding-top: 3em;
      margin-top: 2.3em;
      width: 98%;
      border-radius: 4px;
    }

    textarea {
      height: 150px;
      width: 100%;
      background-color: $white;
      border: solid 1px $gray-400;
      font-size: 16px;
      font-style: italic;
      line-height: 1.43;
      color: $gray-300;
      padding: .5em;
    }

    .hr {
      width: 100%;
      height: 20px;
      border-bottom: 1px solid $gray-500;
      text-align: center;
      margin: 2em 0;
    }

    .hr-middle {
      font-size: 16px;
      font-weight: bold;
      font-family: 'Roboto Condensed';
      line-height: 1.5;
      text-align: center;
      color: $gray-200;
      background-color: $gray-700;
      padding: .2em;
      margin-top: .2em;
      display: inline-block;
      width: 100px;
    }
  }

  h1 {
    color: $purple-200;
  }

  .sidebar {
    background-color: $gray-600;
    padding-top: 2em;
  }

  .toggle-up, .toggle-down {
    width: 20px;
  }

  .toggle-up:hover, .toggle-down:hover {
    cursor: pointer;
  }

  .pause-button {
    background-color: #ffb445 !important;
    color: $white;
    width: 100%;
  }

  .section-header {
    margin-top: 2em;
  }

  .grassy-meadow-backdrop {
    background-image: url('~assets/images/groups/grassy-meadow-backdrop.png');
    width: 472px;
    height: 246px;
  }

  .sleep {
    margin-top: 1em;
  }

  .staff {
    margin-bottom: 1em;

    .title {
      color: #6133b4;
      font-weight: bold;
    }
  }

  .tier-list {
    list-style-type: none;
    padding: 0;
    width: 98%;

    li {
      border-radius: 2px;
      background-color: #edecee;
      border: solid 1px #c3c0c7;
      text-align: center;
      padding: 1em;
      margin-bottom: 1em;
    }

    .tier1 {
      color: #c42870;
    }

    .tier2 {
      color: #b01515;
    }

    .tier3 {
      color: #d70e14;
    }

    .tier4 {
      color: #c24d00;
    }

    .tier5 {
      color: #9e650f;
    }

    .tier6 {
      color: #2b8363;
    }

    .tier7 {
      color: #167e87;
    }

    .moderator {
      color: #277eab;
    }

    .staff {
      color: #6133b4;
    }

    .npc {
      color: $black;
    }
  }

  .staff .title {
    color: #6133b4;
  }

  .moderator .title {
    color: #277eab;
  }

  .bailey .title {
    color: $black;
  }

</style>

<script>
import { mapState } from 'client/libs/store';

import deleteIcon from 'assets/svg/delete.svg';
import copyIcon from 'assets/svg/copy.svg';
import likeIcon from 'assets/svg/like.svg';
import likedIcon from 'assets/svg/liked.svg';
import reportIcon from 'assets/svg/report.svg';
import gemIcon from 'assets/svg/gem.svg';
import questIcon from 'assets/svg/quest.svg';
import challengeIcon from 'assets/svg/challenge.svg';
import informationIcon from 'assets/svg/information.svg';
import questBackground from 'assets/svg/quest-background-border.svg';
import upIcon from 'assets/svg/up.svg';
import downIcon from 'assets/svg/down.svg';

export default {
  data () {
    return {
      icons: Object.freeze({
        like: likeIcon,
        copy: copyIcon,
        report: reportIcon,
        delete: deleteIcon,
        gem: gemIcon,
        liked: likedIcon,
        questIcon,
        challengeIcon,
        information: informationIcon,
        questBackground,
        upIcon,
        downIcon,
      }),
      group: {
        chat: [],
      },
      sections: {
        staff: true,
      },
      staff: [
        {
          name: 'beffymaroo',
          type: 'Staff',
        },
        {
          name: 'lefnire',
          type: 'Staff',
        },
        {
          name: 'Lemoness',
          type: 'Staff',
        },
        {
          name: 'paglias',
          type: 'Staff',
        },
        {
          name: 'redphoenix',
          type: 'Staff',
        },
        {
          name: 'SabreCat',
          type: 'Staff',
        },
        {
          name: 'TheHollidayInn',
          type: 'Staff',
        },
        {
          name: 'viirus',
          type: 'Staff',
        },
        {
          name: 'It\'s Bailey',
          type: 'Moderator',
        },
        {
          name: 'Alys',
          type: 'Moderator',
        },
        {
          name: 'Blade',
          type: 'Moderator',
        },
        {
          name: 'Breadstrings',
          type: 'Moderator',
        },
        {
          name: 'Cantras',
          type: 'Moderator',
        },
        {
          name: 'Daniel the Bard',
          type: 'Moderator',
        },
        {
          name: 'deilann 5.0.5b',
          type: 'Moderator',
        },
        {
          name: 'Dewines',
          type: 'Moderator',
        },
        {
          name: 'Megan',
          type: 'Moderator',
        },
        {
          name: 'shanaqui',
          type: 'Moderator',
        },
      ],
    };
  },
  computed: {
    ...mapState({user: 'user.data'}),
    communityGuidelinesAccepted () {
      return this.user.flags.communityGuidelinesAccepted;
    },
  },
  mounted () {
    // @TODO: Load tavern
  },
  methods: {
    aggreeToGuideLines () {
      // @TODO:
    },
    pauseDailies () {
      // @TODO:
    },
    acceptCommunityGuidelines () {
      this.$store.dispatch('user:set', {'flags.communityGuidelinesAccepted': true});
    },
    toggleSleep () {
      this.$store.dispatch('user:sleep');
    },
  },
};
</script>
