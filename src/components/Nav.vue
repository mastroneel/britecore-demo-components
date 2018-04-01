<template>
  <div id="Nav">
    <a href="#"><img src="../assets/img/logo.png"/> <i class="fa fa-caret-down"></i></a>
    <ul>
      <li><a href="#"><i class="fa fa-umbrella"></i> Policies</a></li>
      <li><a href="#"><i class="fa fa-bolt"></i> Claims</a></li>
      <li><a href="#"><i class="fa fa-user"></i> Contacts</a></li>
      <li><a href="#"><i class="fa fa-bar-chart"></i> Data</a></li>
      <li><a class="selected" href="#"><i class="fa fa-book"></i> Lines</a></li>
      <li><a href="#"><img src="../assets/img/logo-icon.png"/> All Modules</a></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'Nav',
  data () {
    return {

    }
  }
}
</script>

<style lang="scss">
// Variables
$nav-background: #1d4a53;
$selected-nav-background: #29606a;

#Nav {
  background-color: $nav-background;
  height: 50px;
  line-height: 50px;
  img {
    width: 120px;
    margin: 0 0 -5px 20px;
  }
  a {
    text-decoration: none;
    .fa-caret-down {
      margin-left: 20px;
      font-size: 1em;
    }
  }
  ul {
    list-style-type: none;
    display: inline-flex;
    padding: 0;
    margin: 0 0 0 50px;
    li {
      margin: 0 10px;
      a {
        font-size: 0.9em;
        i {
          margin-right: 5px;
        }
        img {
          width: 25px;
        }
      }
      .selected {
        background-color: $selected-nav-background;
        padding: 5px 15px;
        border: 1px solid $selected-nav-background;
        border-radius: 5px;
      }
    }
  }
}

</style>
