<<template>
  <div id="listviewid">
    <section id="js-grid-list" class="grid-list" v-cloak v-show="transactionItems.length > 0">
      <div class="tool-bar">
        <!-- These link buttons use Vue.js to bind click events to change the "layout" variable and bind an active class -->
        <!-- <a class="list-icon" v-on:click="layout = 'list'" v-bind:class="{ 'active': layout == 'list'}" title="List"></a>
                    <a class="grid-icon" v-on:click="layout = 'grid'" v-bind:class="{ 'active': layout == 'grid'}" title="Grid"></a> -->
      </div>
      <!-- Vue.js lets us choose which UL to show depending on the "layout" variable -->
      <ul v-if="layout === 'grid'" class="grid">
        <!-- A "grid" view with photos only -->
        <li v-for="blog in transactionItems">
          <a v-bind:href="blog.url" v-bind:style="{ backgroundImage: 'url(' + blog.image.large + ')' }" target="_blank"></a>
        </li>
      </ul>
      <ul v-if="layout === 'list'" class="list" >
        <li refs="pliitem" v-for="(item, index) in transactionItems" class="pli">
          <div class="columns is-mobile pli">
            <div class="column is-2 fixpadding">
              <figure class="image imagesize">
                <img v-bind:src="item.image.small">
              </figure>
            </div>
            <div class="column is-10 productdescription">
              <div class="columns is-mobile licontent">
                <div class="column is-10-mobile titlepos">
                  <h1 class="titlestyle">{{item.ItemDescription}}</h1>
                </div>
                <div class="column is-2-mobile">
                  <a class="removebtn" @click="removeItem(index)">
                    <div class="block removelocation">
                      <span class="tag is-danger">
                                הסר
                                <button class="delete is-mediun"></button>
                      </span>
                    </div>
                  </a>
                </div>
              </div>
              <div class="columns fittext">
                <div class="column is-12-mobile fulllinedescription">
                  <h3 class="subtitle subtitlestyle">
                  <span class="productdesfirstline">כמות: {{item.ItemQuantity}},</span>
                  <span v-if="item.DiscountPercent >= 0">{{item.DiscountPercent}}% הנחה, </span>
                  <span>סה"כ נטו לשורה: {{item.TotalPrice}} ש"ח</span>
                  <span class="salesnewline" v-if="item.FirstSaleDescription === ' '"> מבצע ראשון למוצר {{item.FirstSaleDescription}}</span>
              </h3>
                </div>
              </div>              
            </div>
          </div>
        </li>
      </ul>
    </section>
  </div>
</template>
<script>
  /* eslint-disable no-undef */
  export default {
    name: 'listview',
    props: ['transactionItems', 'removeitem'],
    data: function() {
      return {
        // The layout mode, possible values are "grid" or "list".
        layout: 'list',
        list: 'list'
      }
    },
    mounted() {
      //  Calculate the height of the list view
      var footerHeight = document.getElementById('ShoppingCart').querySelector('.footer1').clientHeight
      var headerHeight = document.getElementById('headercart').clientHeight
      var screenHeight = window.innerHeight
      var buffer = 20
      // var buffer = 80
      var maxHeight = screenHeight - footerHeight - headerHeight - buffer
      document.getElementsByClassName('list')[0].style.maxHeight = maxHeight + 'px'
    },
    methods: {
      removeItem(index) {
        var msg = this.$t('message.removeitemquestion') + '?'
        if (window.confirm(msg) === true) {
          this.$parent.removeItem(index)
        }
      }
    }
  }
</script>

<style scoped lang='scss'>
  @import '../../common/sass/responsive/mediaqueries.scss';
  @import '../../common/sass/localization/directional.scss';
  /* Starting ListView CSS */
  /* Hide un-compiled mustache bindings until the Vue instance is ready */
  [v-cloak] {
    display: none
  }
  * {
    box-sizing: border-box
  }
  .fittext {
        
  }
  .salesnewline {
    display: block;
  }
  .pli {
    margin: 0px;
    padding: 0px;
  }
  .titlepos {

  }
  .licontent {    
    margin-bottom: 6px;
  }
  .imagefloat {
    //float: $left
    padding: 0px;
    margin: 0px;
  }
  .scrolling {
    overflow-y: scroll;
  }
  .productdescription {
    text-align: $left;
    padding-top: 2px;
  }  
  .productdesfirstline {
    position: relative;
  }
  .fulllinedescription {
    padding-top: 0px;
  }
  .titlestyle {
    font-weight: 700;
    font-size: 30px
  }
  .removebtn {
    margin-bottom: -12px;
    display: flex;
    justify-content: center;
    flex-direction: column;
    height: 100%;
    color: red;
    direction: ltr;
  }
  .subtitlestyle {
    font-weight: 600;
    font-size: 30px;
  }
  .grid-list {
    color: #5e5b64;
    font: 17px/1.3 "Open Sans", sans-serif;
    margin: 0 auto; // max-width: 1024px;
    min-width: 300px;
    padding: 30px;
    text-align: center;
    direction: $dir;
  }
  .grid {
    list-style: none;
    margin: 0 auto;
    padding: 20px;
    text-align: $left;
    width: 100%;
  }
  .imagesize {
    height: 64px !important;
    width: 64px !important;
    margin-right: -3px;
  }
  .list {
    list-style: none;
    margin: 0 auto;
    padding: 0;
    width: 100%;
    overflow-y: auto;
    max-height: 600px
  } // .list li {
  //   border-bottom: 1px solid #fff;
  //   border-radius: 25px;
  //   border: 2px solid black;
  //   overflow: hidden;
  //   padding: 20px;
  //   transition: background 0.2s linear;
  //   background-color: #ddedfb;
  //   margin-bottom: 5px;
  // } 
  .list li {
    position: relative;
    background: #FFF;
    overflow: hidden;
    //border-bottom: 1px solid #f5f5f5;
    min-height: 68px;
    /* padding-left: 5px; */
    padding-right: 85px;
  }
  @media #{$mobileportrait} {
    .subtitlestyle {
      font-weight: 400;
      font-size: 14px;
    }
    .fixpadding {
      padding: 0px;
    }
    .titlestyle {
      font-weight: 700;
      font-size: 15px
    }
    .imagefloat {
      float: inherit
    }
    #trashicon {
      transform: scale(0.8);
    }
    .imagesize {
      height: 64px;
      width: 64px;
    }
    .list {
      list-style: none;
      margin: 0 auto;
      padding: 2px;
      width: 100%;
      overflow-y: auto;
      max-height: 300px
    }
    .grid-list {
      font: 17px/1.3 "Open Sans", sans-serif;
      margin: 6px; // max-width: 1024px;
      padding: 2px;
      min-width: 300px;
      text-align: center;
      border: 1px solid #FFF;
      background: #FFF;
    }
    .list li {
      position: relative;
      background: #FFF;
      overflow: hidden;
      border-bottom: 2px solid #f5f5f5;
      min-height: 68px;
      /* padding-left: 5px; */
      margin-bottom: 2px;
      padding-right: 1px;
      &:last-child {
        border-bottom: none;
        margin-bottom: 0;
      }
    }
  } // @media (max-width: 1024px) {
  //   .list {
  //     list-style: none;
  //     margin: 0 auto;
  //     padding: 0;
  //     width: 100%;
  //     overflow-y: auto;
  //     max-height: 800px
  //   }
  // }
  // @media screen and (max-width: 500px) {
  //   .list {
  //     list-style: none;
  //     margin: 0 auto;
  //     padding: 0;
  //     width: 100%;
  //     overflow-y: auto;
  //     max-height: 700px
  //   }
  // }
  //a,
  //a:visited,
  // a:hover 
  //   color: #389dc1;
  //   display: block;
  //   outline: none;
  //   text-decoration: none;
  // a:hover 
  //  text-decoration: none;
  // .btnremove
  //    border-radius: 25px;
  //    background: red;
  //    padding: 20px;
  //    width: 100px;
  //    height: 100px;
  //    margin-bottom: 10%
  //    float: left
  // .tool-bar {
  //   background-color: white;      
  //   background-position: top left;
  //   background-repeat: no-repeat;
  //   background-size: contain;
  //   // box-shadow: 0 1px 1px #ccc;
  //   line-height: 1;
  //   margin: 0 auto;
  //   padding: 10px;
  //   position: relative;
  //   text-align: $textalign;
  //   width: 100%;
  // }
  // .tool-bar a {
  //   background: rgba( 255, 255, 255, 0.50 ) center center no-repeat;
  //   border: 1px solid #e4e4e4;
  //   border-right: 1px solid #908f8f;
  //   border-bottom: 1px solid #908f8f;
  //   cursor: pointer;
  //   display: inline-block;
  //   height: 32px;
  //   margin-right: 5px;
  //   text-decoration: none;
  //   width: 32px;
  // }
  // .tool-bar a:hover {
  //   border: 1px solid #e4e4e4;
  // }
  // .tool-bar a.active {
  //   background-color: rgba( 0, 0, 0, 0.50 );
  //   border: 1px solid #908d8d;
  //   border-right: 1px solid #e2e2e2;
  //   border-bottom: 1px solid #e2e2e2;
  // }
  // .tool-bar a.active:hover {
  //   cursor: default;
  // }
  //.tool-bar a.list-icon 
  //  background-image: url( "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyBpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMC1jMDYwIDYxLjEzNDc3NywgMjAxMC8wMi8xMi0xNzozMjowMCAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNSBXaW5kb3dzIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkYzNkFCQ0ZBMTBCRTExRTM5NDk4RDFEM0E5RkQ1NEZCIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkYzNkFCQ0ZCMTBCRTExRTM5NDk4RDFEM0E5RkQ1NEZCIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6RjM2QUJDRjgxMEJFMTFFMzk0OThEMUQzQTlGRDU0RkIiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6RjM2QUJDRjkxMEJFMTFFMzk0OThEMUQzQTlGRDU0RkIiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz7h1bLqAAAAWUlEQVR42mL8////BwYGBn4GCACxBRlIAIxAA/4jaXoPEkMyjJ+A/g9MDJQBRhYg8RFqMwg8RJIUINYLFDmBUi+ADQAF1n8ofk9yIAy6WPg4GgtDMRYAAgwAdLYwLAoIwPgAAAAASUVORK5CYII=" );
  //.tool-bar a.grid-icon  
  //  background-image: url( "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyBpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMC1jMDYwIDYxLjEzNDc3NywgMjAxMC8wMi8xMi0xNzozMjowMCAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNSBXaW5kb3dzIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOjBEQkMyQzE0MTBCRjExRTNBMDlGRTYyOTlBNDdCN0I4IiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOjBEQkMyQzE1MTBCRjExRTNBMDlGRTYyOTlBNDdCN0I4Ij4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6MERCQzJDMTIxMEJGMTFFM0EwOUZFNjI5OUE0N0I3QjgiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6MERCQzJDMTMxMEJGMTFFM0EwOUZFNjI5OUE0N0I3QjgiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz4MjPshAAAAXklEQVR42mL4////h/8I8B6IGaCYKHFGEMnAwCDIAAHvgZgRyiZKnImBQsACxB+hNoDAQyQ5osQZIT4gH1DsBZABH6AB8x/JaQzEig++WPiII7Rxio/GwmCIBYAAAwAwVIzMp1R0aQAAAABJRU5ErkJggg==" );
  // .grid li {
  //   display: inline-block;
  //   position: relative;
  //   width: 50%;
  // }
  // .grid li:after 
  //   content: "";
  //   display: block;
  //   padding-bottom: 52%;
  // .grid li a 
  //   background-position: center;
  //   background-repeat: no-repeat;
  //   background-size: cover;
  //   border: 2px solid #fff;
  //   height: 99%;
  //   position: absolute;
  //   width: 98%;
  // .grid li a:hover 
  //   border: 2px solid #000;
  //   box-shadow:         inset 0 0 90px -10px rgba( 0, 0, 0, 1 );
  //   -moz-box-shadow:    inset 0 0 90px -10px rgba( 0, 0, 0, 1 );
  //   -webkit-box-shadow: inset 0 0 90px -10px rgba( 0, 0, 0, 1 );
  /* List layout */
  // .list li:hover 
  //   background-color: #fff;
  // .list li a 
  //   display: table;
  //   width: 100%;
  // .list li img 
  //   border: none;
  //   display: table-cell;
  //   height: 120px;
  //   vertical-align: middle;
  //   width: 120px;
  // .list li:hover p 
  //   color: #0096d4;
  // .list li p 
  //   color: #000;
  //   display: table-cell;
  //   font-weight: 700;
  //   padding: 0 15px 0 15px;
  //   vertical-align: middle;
  //   width: 100%;
  // .list {
  //   list-style: none;
  //   margin: 0 auto;
  //   padding: 0;
  //   width: 100%;
  //   overflow-y: auto;
  //   max-height: 600px
  // }
  /* End ListView CSS */
</style>
