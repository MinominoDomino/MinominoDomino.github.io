define([
    "mui/util",
    "mui/component",
    "text!mui/dialog.tpl",

], function (Util, Component, Template) {
    "use strict";
    /**
     * A module representing a dialog.
     */
    var Dialog = {};
    /**
    * A module representing a component model.
    * @extend Component
    * @exports Dialog
    */
    Dialog.Model = Component.Model.extend({
        initialize: function (opts) {
            Component.Model.prototype.initialize.apply(this, [opts]);
            this.set({
                ctype: "mui/dialog",
                zIndex: 1,
                display: "none",
                paddingTop: "100px",
                position: "absolute", //fixed
                left: "50%",
                top: "50%",
                width: "100%",
                height: "100%",
                overflow: "auto",
                backgroundColor: "rgba(63,81,181,100)",

                dragable: true,
                isClicked: false,

                isFirstMove: true,

            });
        }
    });
    /**
     * A class used to manage style in a component.
     * @class
     * @extends Component.Style
     * @memberof module:mui/dialog
     * @param {object} opts - initialized options.         
     */
    Dialog.Style = Component.Style.extend({
        initialize: function (opts) {
            Component.Style.prototype.initialize.apply(this, [opts]);
        }
    });
    /**
    * A module representing a component view.
    * @extend Component
    * @exports Dialog
    */
    Dialog.View = Component.View.extend({
        /**
         * Represents an HTML fragment template.<br> HTML 템플릿.
         * @memberof module:mui/dialog.View
         */
        initialize: function (opts) {
            if (Util.isEmpty(this.model)) {
                this.model = new Dialog.Model();
            }

            /** call super class */
            Component.View.prototype.initialize.apply(this, [opts]);

            this.listeners.mousedown = this.onMouseEvent;
            this.listeners.mousemove = this.onMouseEvent;
            this.listeners.mouseup = this.onMouseEvent;
        },
        template: Template,

        onMouseEvent: function(sender, event) {
            var eventObj = window.event? window.event : event;

            if(event.type === "mousedown") {
                $(sender.selector).attr("draggable", "true");

                var oriLeft = parseInt($(sender.selector).css("left"));
                var oriTop = parseInt($(sender.selector).css("top"));
                $(sender.selector).attr("oriLeft", oriLeft - eventObj.clientX);
                $(sender.selector).attr("oriTop", oriTop - eventObj.clientY);
            
                if(eventObj.preventDefault)eventObj.preventDefault(); 

            } else if(event.type === "mouseup") {
                $(sender.selector).attr("draggable", "false");
                $(sender.selector).removeAttr("oriLeft");
                $(sender.selector).removeAttr("oriTop");
            } else if(event.type === "mousemove") {
                var $selector = $(sender.selector);

                if($selector.attr("draggable") === "true") {
                    var left = (parseInt(eventObj.clientX) + parseInt($(sender.selector).attr("oriLeft")));
                    var top = (parseInt(eventObj.clientY) + parseInt($(sender.selector).attr("oriTop")));
                    $(sender.selector).css("top",top);
                    $(sender.selector).css("left", left);
                }
            }
        },
        afterRender: function () {
            var self = this;
            var $selector = $(this.selector);
            $selector.css({
                backgroundColor: self.model.get("backgroundColor"),
                width: self.model.get("width"),
                height: self.model.get("height"),
                position: self.model.get("position"),
                left: self.model.get("left")-100,
                top: self.model.get("top")
            });
        }
    });

    return Dialog;
});
