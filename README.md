<!--
 * @Brief:Bootstrap Learning
 * @LastEditors: Jerry Lee
 * @LastEditTime: 2020-07-22 19:19:51
-->

# Bootstrap_For_Learning

##Bootstrap 的学习：
###Bootstrap 的使用方法：

        <!-- 引入 Bootstrap.css 样式化文件 -->
        <link rel="stylesheet" href="bootstrap-3.3.7-dist/css/bootstrap.min.css"/>
        <!-- npm环境 -->
        <script src="bootstrap-3.3.7-dist/js/jquery-1.12.4.min.js"></script>
        <!-- 引入bootstrap.js文件 -->
        <script src="bootstrap-3.3.7-dist/js/bootstrap.min.js"></script>

Bootstrap 提供了一套响应式、移动设备优先的流式栅格系统，随着屏幕或视口（viewport）尺寸的增加，系统会自动分为最多 12 列。它包含了易于使用的预定义类，还有强大的 mixin 用于生成更具语义的布局。
###container-fluid 流体容器

        width:100%的响应式布局

###container 固定容器

        @media媒体查询:
                浏览器窗口大小          width：
                >=1200px               1170px
                >=992px                970px
                >=768px                750px
                <768px                 auto

###栅格系统

        Bootstrap 提供了一套响应式、移动设备优先的流式栅格系统，随着屏幕或视口（viewport）尺寸的增加，系统会自动分为最多 12 列。它包含了易于使用的预定义类，还有强大的 mixin 用于生成更具语义的布局。

        .container > .row > .col-lg-(num)
                num为多少，占据多少个栅格。

        栅格源码分析：

                mixin/grid.less
                mixin/grid-framework.less
                grid.less
                varibales.less

                grid.less源码分析：
                        1.// 固定容器和流体容器通用样式
                                .container-fixed()函数：
                                        .container-fixed(@gutter: @grid-gutter-width) {         //@grid-gutter-width 30px
                                                margin-right: auto;
                                                margin-left: auto;
                                                padding-left: floor((@gutter / 2));             //15px
                                                padding-right: ceil((@gutter / 2));             //15px
                                                &:extend(.clearfix all);                        //清除浮动，防止父容器塌陷
                                        }

                        2.// 固定容器
                                .container {
                                        // 调用公共样式
                                        .container-fixed();
                                        // @screen-sm :
                                        // @screen-sm-min: @screen-sm;
                                        @media (min-width: @screen-sm-min) {
                                                // @container-tablet: (720px + @grid-gutter-width);
                                                // @container-sm: @container-tablet; 768px;
                                                width: @container-sm;
                                        }

                                        // @screen-md:                  992px;
                                        // @screen-md-min:              @screen-md;
                                        @media (min-width: @screen-md-min) {
                                                // @container-desktop: (940px + @grid-gutter-width);
                                                // @container-md: @container-desktop;   970px
                                                width: @container-md;
                                        }

                                        // @screen-lg:                  1200px;
                                        // @screen-lg-min:              @screen-lg;
                                        @media (min-width: @screen-lg-min) {
                                                // @container-large-desktop: (1140px + @grid-gutter-width);
                                                // @container-lg: @container-large-desktop;   1170px
                                                width: @container-lg;
                                        }
                                }

                        3.//行
                                .row {
                                        // .make-row(@gutter: @grid-gutter-width) { // @grid-gutter-width:30px
                                        //     margin-left: ceil((@gutter / -2));   //-15px
                                        //     margin-right: floor((@gutter / -2)); //-15px
                                        //     &:extend(.clearfix all);             //清除浮动
                                        // }
                                        .make-row();
                                }
                        4.//列
                        第一步：
                                .make-grid-columns();
                        第二步：
                                // 手机屏幕布局样式
                                .make-grid(xs);

                                // Small grid col 平板布局样式
                                @media (min-width: @screen-sm-min) {
                                .make-grid(sm);
                                }

                                // Medium grid 中屏 PC 布局样式
                                @media (min-width: @screen-md-min) {
                                .make-grid(md);
                                }

                                // Large grid 大屏 PC 布局样式
                                @media (min-width: @screen-lg-min) {
                                .make-grid(lg);
                                }
                                        grid-framework.less源码解析：
                                                //制作column的通用布局样式
                                                .make-grid-columns() {
                                                }

                                                //开启column的float为left；设置width/left/right/margin-left根据index的值而变化，取值为index/12
                                                .make-grid(@class) {
                                                        //开启栅格的float属性
                                                        .float-grid-columns(@class);

                                                        //设置widh的宽度根据index值的变化而变化，width为index/12
                                                        // .col-@{class}-@{index} {
                                                        //     width: percentage((@index / @grid-columns));
                                                        // }
                                                        .loop-grid-columns(@grid-columns, @class, width);

                                                        // 设置栅格position的right位置，根据index的变化而变化
                                                        <!-- 列右排序 -->
                                                        .loop-grid-columns(@grid-columns, @class, pull);

                                                        // 设置栅格position的left位置，根据index的变化而变化
                                                        <!-- 列左排序 -->
                                                        .loop-grid-columns(@grid-columns, @class, push);

                                                        // 设置栅格margin-left根据index的变化而变化
                                                        .loop-grid-columns(@grid-columns, @class, offset);
                                                }
                        列的一般样式汇总为：
                                .col-@{class}-@{index}{
                                        position: relative;
                                        min-height: 1px;
                                        padding-left: ceil((@grid-gutter-width / 2)); //15px
                                        padding-right: floor((@grid-gutter-width / 2)); //15px

                                        //.float-grid-columns(@class);
                                        float:left;

                                        //.loop-grid-columns(@grid-columns, @class, width);
                                        width:percentage((@index / @grid-columns));

                                        //.loop-grid-columns(@grid-columns, @class, pull);
                                        right: percentage((@index / @grid-columns));


                                        //.loop-grid-columns(@grid-columns, @class, push);
                                        left: percentage((@index / @grid-columns));

                                        //.loop-grid-columns(@grid-columns, @class, offset);
                                        margin-left: percentage((@index / @grid-columns));
                                }

###Bootstrap 实例的运用

        在 bootstrap 组件中找到需要的组件，复制代码。

###自定义栅格系统

        从源码中复制下列文件到自定义系统文件夹
                mixin/clear.less
                mixin/grid.ess
                mixin/grid-framework.less
                grid.less
                variables.less
        在系统文件夹新建.less文件，将上述文件引入：
        写：
                @import "grid.less";
                @import "variables.less";
                @import "mixin/grid.less";
                @import "mixin/grid-framework.less";
                @import "mixin/clearfix.less";
                @import "mixin/grid-framework.less";
        编译成css文件。
        用法和原bootstrap栅格系统一致。

###响应式工具：

        引入以下两个文件
        mixin/responsive-visibility.less
        responsive-utilities.less

###栅格盒模型设计的精妙之处

        容器    上两边具有15px的padding
        行      两边具有-15px的margin
        列      两边具有15px的padding

        为了维护槽宽的规则：
                列两边必须要右15px的padding
                为了能使列嵌套行，行两边必须有-15px的margin
                为了能使容器包裹行，容器两边必须有15px的padding
