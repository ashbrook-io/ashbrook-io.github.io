---
layout: post
published: false
title: Adding Chart Labels with Chart JS to Bar Charts and Stacked Bar Charts
---
I'll preface this by saying that you probably should just use the the official data label plugin from chartjs. You can find that [here](https://github.com/chartjs/chartjs-plugin-datalabels). I didn't want to add another dependency for charting, however, so I decided to just roll my own setup. Here are two ways I did this.

I will also add a caveat that I am not a chartjs expert. I do use it for my charting, but mostly that is because it handles combo charts easier than some of the other ones I tried. I don't have to touch these charts too much and I don't mind refreshing my memory every time I go to make updates.

## Plugin

Previously, I had a bar chart that showed some value for every day of the month and I needed to add a vertical line that showed where the 'Sunday's were to make it easier for people to see the week breaks. Probably other ways to do this visually, but the request from the users was just to add a verical dashed line with 'Sun' up top. After some research, I settled on doing this with a plugin. So, I first tackled this approach that same way. Below is some annotated code. You can take all the comments out and that's what is running right now for regular bar charts. There are various events to put this on. I am using afterRender because it doesn't 'run' as frequently. But this does mess with your hover and other animations. The easiest way to avoid this issue is to add `events: []` to your options. This basically skips any events like hover and other items. Now you can leave the events and then go with afterDatasetsDraw, but in my experience that seems to fire a bunch of times. I'm not sure why. In either case, if you don't want to remove your events and hover, just change afterRender to afterDatasetsDraw. You can see more about the event hooks [here](https://blog.larapulse.com/javascript/creating-chart-js-plugins). I'm not sure if I'm missing things or something, but I couldnt' find the same ince information from taht blog on the documentation site itself.

### Regular Bar Charts

```JavaScript
// i have this in another file and am actually using svelte on this site, so i import this. But you could also just include the object value in your plugin value in options. I'm using inline plugins in this case because i have some customizations for each one for formatting.
export const BarChartLabels = {
    afterRender: function (chart, options) {
        // this is some item i had in here for looking at mobile, i figured i'd leave it here as a comment in case anyone wanted to see how you could check the screen width. The challenge on this seemed to be that the 'screen' would look at your device screen, so if you were shrinking the width vs changing the device in chrome, this will still show the same thing. But since most css rules are going to use the screen width to determine things, I wanted to do that the same way.
        // if (window.screen.width < 575.99) {
        //   console.log('too small for labels', window.screen.width)
        //   return
        // }
        // i am not using any options, but plugins will get passed a chart instance by default
      // i think you can skip the argument and just use 'this' but i normally try and avoid that
        let chartInstance = chart
        let ctx = chartInstance.chart.ctx
        // aligning the text in the center based on the point we'll put it at. 
        ctx.textAlign = 'center'

        // this is just a function to format the label. in this case, i have bar charts that are positive and negative and have various dollar amounts. i want to just abbreviate things to 2.33M for millions and 120k for thousands. i just leave it alone if it's bigger or smaller. I also wrap it in parentheses if it's negative and remove the - symbol.
        // NOTE!!!! I am using a function i didn't include here called number_format. That's just a utility function that formats a number to a specific string format. you don't need it here, but i have it here as part of some common practice with some other labels so i left it. 
        function formatLabel(value) {
            let retval = '$'
            let absvalue = Math.abs(value)
            if (absvalue >= 1000000) {
                retval += (absvalue / 1000 / 1000).toFixed(2) + 'M'
            } else {
                retval += +number_format(absvalue / 1000) + 'K'
            }
            if (value < 0) {
                retval = `(${retval})`
            }
            return retval
        }

        // so i am processing all of the datasets in the chart. But in reality I am only using this for one data set so I could just look for the first. But just in case you have a non stacked bar chart, processing them all should work this way.
        chartInstance.data.datasets.forEach(function (dataset, i) {
            // you need to get the meta for each item so you can get positions, the x and y are in there. there are other ways to access the meta as you can actually just walk down the tree, but this was in one of the examples I used and it seemed to work quite well for me so there you go. we'll get the meta for the index of the dataset in question passed in above
            let meta = chartInstance.controller.getDatasetMeta(i)
            // walk through our meta as this will have each of the points in it. i'm not sure what object this is, but we're calling it bar here as some example i found that did stuff to bar charts did this. again we are passing in the index for this item as well since we'll need to go back to the dataset, to it's data, and get the actual value out using this index.
            meta.data.forEach(function (bar, index) {
                // get label string, we go into the dataset that has this meta and get the index for this particular data element, now we have the value to put on our label
                let data = formatLabel(dataset.data[index])
                // for me, i have some negative values, so a chart with a 0 in the middle and things fall above and below the line. i'll need to know that as the offset for something on the bottom will be different than something on the top. 
                const isNeg = dataset.data[index] < 0
                // set the baseline for the text to top or bottom based on whether it's positive or negative
                ctx.textBaseline = isNeg ? 'top' : 'bottom'
                // change color to red if it's negative, otherwise make it black
                ctx.fillStyle = isNeg ? '#ff0000' : '#000000'
                // here we scale spacing and font based on the size of the bar. there may be other ways to do this, but chartjs seems to handle resizing the width of bar charts just fine, so i am just using some relative numbers based on the width of the bar itself. Now, since they all resize, you can technically grab *any* of the bars on the chart and check the width because they are all the same. the name 'width' is a little decieving here because i'm really just getting something like a custom ratio multiplier or something. The idea was just to use some ratio of the bar width for font sizes so things looked more consistent vs having a giant bar and tiny text or having it the same size. this app runs on desktop and mobile so some scaling helps. may be a better way to do this, but when i played around with some of the responsive stuff with chartjs, it didn't let me fine tune it where i wanted to be and this worked just fine. 
                // these numbers were just trial and error, but just trying a size and then tuning it a bit worked fine. 
                let width = bar._model.width / 4
                // if we have a negative value
                let yoffset = (width * (isNeg ? -2 : 1)) / 3
                let fontsize = width > 15 ? 15 : width
                ctx.font = `${fontsize}px Arial`
                //add label to bar
                ctx.fillText(data, bar._model.x, bar._model.y - yoffset)
            })
        })
    },
}
```

Now, in my case I wanted to scale the size and position of the label

```JavaScript
export const StackedBarChartLabels = {
    afterRender: function (chart, options) {
        let chartInstance = chart
        let ctx = chartInstance.chart.ctx
        ctx.textAlign = 'center'

      	//function to format value...
        function formatLabel(value) => value;
        let datasets = chart.data.datasets;
            //get the width of any one of the bar charts for spacing...
            //scale spacing and font based on the size of the bar, use the first model as it should be a regular bar chart
            let firstmetakey = Object.keys(datasets.map(x=>x._meta)[0])[0]
            let width = datasets.map(x=>x._meta[firstmetakey]).map(a=>a.data)[0][0]._model.width / 4
            let totalsIdx = datasets.length-1; //should always be last dataset
            let dataset = datasets[totalsIdx]
            let meta = dataset._meta[firstmetakey]
            meta.data.forEach(function (bar, index) {
                //get label string
                let data = dataset.data[index] < 100 ? dataset.data[index] : formatLabel(dataset.data[index])
                // console.log(data,bar._model.x, bar._model.y);
                //reposition based on positive/negative
                ctx.fillStyle = '#000000'
                let yoffset = (width * (1)) / 2
                let fontsize = width > 15 ? 15 : width
                if(`${data}`.length < 3){
                    fontsize = fontsize * 1.5
                    yoffset = yoffset * 1.5
                }
                ctx.font = `${fontsize}px Arial`
                //add label to bar
                ctx.fillText(data, bar._model.x, bar._model.y - yoffset)
            })
        // })

        // if (window.screen.width < 575.99) {
        //   console.log('too small for labels', window.screen.width)
        //   return
        // }
        // let chartInstance = chart
        // let ctx = chartInstance.chart.ctx
        // ctx.textAlign = 'center'

        // function formatLabel(value) {
        //     let retval = '$'
        //     let absvalue = Math.abs(value)
        //     if (absvalue >= 1000000) {
        //         retval += (absvalue / 1000 / 1000).toFixed(2) + 'M'
        //     } else {
        //         retval += +number_format(absvalue / 1000) + 'K'
        //     }
        //     if (value < 0) {
        //         retval = `(${retval})`
        //     }
        //     return retval
        // }

        // //chartInstance.data.datasets.forEach(function (dataset, i) {
        // let i = 2
        // let dataset = chartInstance.data.datasets[i]
        //     let meta = chartInstance.controller.getDatasetMeta(i)
        //     meta.data.forEach((item,index)=>console.log(item,index));
        //     meta.data.forEach(function (bar, index) {
        //         //get label string
        //         let data = formatLabel(dataset.data[index])
        //         //reposition based on positive/negative
        //         const isNeg = dataset.data[index] < 0
        //         ctx.textBaseline = isNeg ? 'top' : 'bottom'
        //         ctx.fillStyle = isNeg ? '#ff0000' : '#000000'
        //         //scale spacing and font based on the size of the bar
        //         let width = bar._model.width / 4
        //         let yoffset = (width * (isNeg ? -2 : 1)) / 3
        //         let fontsize = width > 15 ? 15 : width
        //         ctx.font = `${fontsize}px Arial`
        //         //add label to bar
        //         ctx.fillText(data, bar._model.x, bar._model.y - yoffset)
        //     })
        // })
        // let chartInstance = this.chart
        // let ctx = chartInstance.chart.ctx
        // ctx.textAlign = 'center'

        // function formatLabel(value) {
        //     let retval = '$'
        //     let absvalue = Math.abs(value)
        //     if (absvalue >= 1000000) {
        //         retval += (absvalue / 1000 / 1000).toFixed(2) + 'M'
        //     } else {
        //         retval += +number_format(absvalue / 1000) + 'K'
        //     }
        //     if (value < 0) {
        //         retval = `(${retval})`
        //     }
        //     return retval
        // }
        // this.data.datasets.filter(x=>x.label === "Total").forEach(function (dataset, i) {
          
        //     let meta = dataset._meta[3]
        //     meta.data.forEach(function (bar, index) {
        //         //get label string
        //         let data = formatLabel(dataset.data[index])
        //         // console.log(data,bar._model.x, bar._model.y);
        //         //reposition based on positive/negative
        //         ctx.fillStyle = '#000000'
        //         //scale spacing and font based on the size of the bar
        //         let width = bar._model.width / 4
        //         let yoffset = 5//(width * (isNeg ? -2 : 1)) / 3
        //         let fontsize = 15//width > 15 ? 15 : width
        //         ctx.font = `${fontsize}px Arial`
        //         //add label to bar
        //         ctx.fillText(data, bar._model.x, bar._model.y - yoffset)
        //     })
        // })
    }
}
```



## A New Post

Enter text in [Markdown](http://daringfireball.net/projects/markdown/). Use the toolbar above, or click the **?** button for formatting help.
