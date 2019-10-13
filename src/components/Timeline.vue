<template>
    <div class="page">
        <div class="timeline">
            <div class="timeline__group">
                <span class="timeline__year">2019</span>
                <div class="timeline__box">
                    <div class="timeline__date">
                        <span class="timeline__day">13</span>
                        <span class="timeline__month">Oct</span>
                    </div>
                    <div class="timeline__post">
                        <div class="timeline__content">
                            <ul>
                                <li>
                                    
                                    Inspecting ressources and showing the price differences for two cities.</li>
                                <li>Using capacity to show how much goods can be transported.</li>
                            </ul>
                        </div>
                    </div>
                </div>
                <div class="timeline__box">
                    <div class="timeline__date">
                        <span class="timeline__day">11</span>
                        <span class="timeline__month">Oct</span>
                    </div>
                    <div class="timeline__post">
                        <div class="timeline__content">
                            <ul>
                                <li>
                                    Initial setup and start of the project. 
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    
}
</script>
<style lang="less">

.timeline {
  --uiTimelineMainColor: var(--timelineMainColor, #222);
  --uiTimelineSecondaryColor: var(--timelineSecondaryColor, #fff);

  position: relative;
  padding-top: 3rem;
  padding-bottom: 3rem;
}

.timeline:before {
  content: "";
  width: 4px;
  height: 100%;
  background-color: var(--uiTimelineMainColor);

  position: absolute;
  top: 0;
}

.timeline__group {
  position: relative;
}

.timeline__group:not(:first-of-type) {
  margin-top: 4rem;
}

.timeline__year {
  padding: .5rem 1.5rem;
  color: var(--uiTimelineSecondaryColor);
  background-color: var(--uiTimelineMainColor);

  position: absolute;
  right: 0;
  top: 0;
}

.timeline__box {
  position: relative;
}

.timeline__box:not(:last-of-type) {
  margin-bottom: 30px;
}

.timeline__box:before {
  content: "";
  width: 100%;
  height: 2px;
  background-color: var(--uiTimelineMainColor);

  position: absolute;
  right: 0;
  z-index: -1;
}

.timeline__date {
  min-width: 65px;
  position: absolute;
  right: 0;

  box-sizing: border-box;
  padding: .5rem 1.5rem;
  text-align: center;

  background-color: var(--uiTimelineMainColor);
  color: var(--uiTimelineSecondaryColor);
}

.timeline__day {
  font-size: 2rem;
  font-weight: 700;
  display: block;
}

.timeline__month {
  display: block;
  font-size: .8em;
  text-transform: uppercase;
}

.timeline__post {
  padding: 1.5rem 2rem;
  padding-left: 0;
  border-radius: 2px;
  border-right: 3px solid var(--uiTimelineMainColor);
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, .12), 0 1px 2px 0 rgba(0, 0, 0, .24);
  background-color: var(--uiTimelineSecondaryColor);
}

@media screen and (min-width: 641px) {

  .timeline:before {
    right: 40px;
  }

  .timeline__group {
    padding-top: 55px;
  }

  .timeline__box {
    padding-right: 90px;
  }

  .timeline__box:before {
    top: 50%;
    transform: translateY(-50%);
  }

  .timeline__date {
    top: 50%;
    margin-top: -27px;
  }
}

@media screen and (max-width: 640px) {

  .timeline:before {
    right: 0;
  }

  .timeline__group {
    padding-top: 40px;
  }

  .timeline__box {
    padding-right: 20px;
    padding-top: 70px;
  }

  .timeline__box:before {
    top: 90px;
  }

  .timeline__date {
    top: 0;
  }

  .timeline__post {
    margin-top: 15px;
  }
}

.timeline{
  --timelineMainColor: #4557bb;
  font-size: 16px;
}
</style>