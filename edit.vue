    <Row>
        <FormItem label="特殊休息日期设置">
         <div class="flex">
           <Input prefix="" type="text" @on-focus="openMultiByDrop($event)" @on-blur="closeMultiByDrop"  v-model="calendar5.display" readonly/>
           </div>
        </FormItem>
               <transition name="fade">
      <div id="calendarModel" class="calendar-dropdown" :style="{'left':calendar5.left+'px','top':calendar5.top+'px'}" v-show="calendar5.show">
        <calendar ref="calendarChild" :zero="calendar5.zero" :disabled="calendar5.disabled" :lunar="calendar5.lunar" :value="calendar5.value" :multi="calendar5.multi" @select="calendar5.select"></calendar>
    </div>
    </transition>
    </Row>
          <FormItem v-show="speaclTagshow">
            <Tag v-for="(item,index) in tagCount" :key="index" type="border" :name="index" closable :kI="item.k1" :ke="item.k2" @on-close="handleClose2($event,index,item.k1,item.k2,item.year,item.month,item.days)"  color="success" >{{item.year}}-{{item.month}}-{{item.days}}</Tag>
        </FormItem>
        
        <script>
           import calendar from './calendar.vue'
        export default{
            components: {
    UploadVideo,calendar
  },
  
    data(){
      return{
          speaclTagshow:false,//特殊休息日期默认不显示
      tagCount:[],//tag标签-特殊休息日期
      displayModleValue:[],
      testObj:null, //上传到后台数据
      modelTestWarp:[],
      dateTest:[],
      calendar5:{  //设置特殊休息日期
                display:"",
                multi:true,
                show:false,
                zero:true,
                value:[], //默认日期
                disabled:[], //默认日期
                lunar:true, //显示农历
               
                select:(value,days)=>{
                console.log('value:',value)
                // console.log('days',days)
                    let displayValue = []
                    var modelTagGroup = []
                    var showGroup = []
                    var modelTest = []  //测试
                    value.forEach(v=>{
                        // displayValue.push(v[0]+"-"+(v[1])+"-"+v[2]);
                      this.speaclTagshow = true;
                      // modelTagGroup.push(v[0]+"-"+(v[1])+"-"+v[2])
                      var model = {
                         year:v[0],
                         month:v[1],
                         days:v[2],
                         k1:v[3], //获取日历组件的下标
                         k2:v[4]//获取日历组件的下标
                      }
                      
                      this.testObj = {[v[0]+"-"+v[1]+"-"+v[2]]:{"k1":v[3],"k2":v[4]}}
                     modelTest.push( this.testObj )
                      // console.log('test',test)
                      modelTagGroup.push(model)  //tag标签存储值
                      displayValue.push(model) //input输入框显示的值
                    })
                    // console.log('displayValue',displayValue);
                      //  console.log('modelTest',modelTest)
                  
                   for(var i = 0;i<displayValue.length;i++){
                           showGroup.push(parseInt(displayValue[i].year)+"-"+parseInt(this.checkTimeDate(displayValue[i].month))+"-"+parseInt(this.checkTimeDate(displayValue[i].days)));
                   }           
                      for(var j = 0;j<modelTest.length;j++){
                         var a = modelTest[j];
                         for(var i in modelTest[j]){
                             this.dateTest.push(i)
                         }
                      }
              
                    var ModleshowGroup = Array.from(showGroup);
                    console.log('ModleshowGroup',ModleshowGroup)
                    this.displayModleValue = displayValue;
                    this.calendar5.display=ModleshowGroup.join(",");
                     this.unableDays = ModleshowGroup.join(","); //存储不可用时间
                     this.formSendData.unableDays = ModleshowGroup.join(",");
                     console.log('不可用时间:',this.storeInfo.unableDays)
                    this.tagCount = Array.from(modelTagGroup)
                
                    // this.calendar5.show=false;
                    this.calendar5.value=value;
                    this.calendar5.show = false;
                    
                }
            },
            dateItemGroup:[],
            DateTimeGroup:[],//存储营业时间模块
            unableWeeksFroup:[],//存储不可用星期
            timesGroup:[],//存储营业时间
            unableWeeks:'',//不可用星期
            unableDays:'',//不可用时间
            times:'00:00-23:30',//营业时间
              startTimeGroup:'',
            endTimeGroup:'',
            endUnabled:false,
           dateModelDays:'' //不可用
      
      }
    }
        methods:{
         //选择特殊休息日期
 openMultiByDrop(e){
  //  console.log(e)
   var _this = this;
            _this.calendar5.show=true;
            _this.calendar5.left=e.target.offsetLeft+19;
            _this.calendar5.top=e.target.offsetTop+40;
            e.stopPropagation();
            window.setTimeout(()=>{
                document.addEventListener("click",(e)=>{
                    // _this.calendar5.show=false;
                    // console.log(e)
                    document.removeEventListener("click",()=>{},false);
                },false);
            },1000)
        },
        //关闭选择休息日期
        closeMultiByDrop(e){
          var _this = this;
         _this.calendar5.show=false;
        },
        //删除特殊日期标签
        handleClose2(event,name,k1,k2,year,month,days){
          //  console.log(name,k1,k2)
          var _this = this;
          const index = this.tagCount.indexOf(name);
            this.tagCount.splice(index, 1);
            this.displayModleValue.splice(index, 1);
            console.log(this.displayModleValue)
              this.calendar5.display=this.displayModleValue.join(",");
              // this.storeInfo.unableDays =  this.displayModleValue.join(",");
              // console.log(_this.$refs.calendarChild)
              _this.$refs.calendarChild.closeSelectFunc(k1,k2,year,month,days);
        },
      
        
        }  
        
        }
        
        </script>
