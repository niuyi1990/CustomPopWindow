# CustomPopWindow
##自定义PopWndow，像AlertDialog一样的链式表达式
###使用说明：
        public static final float DARK_ALPHA = 0.4F;
        public static final float BRIGHT_ALPHA = 1.0F;

        View contentView = LayoutInflater.from(this).inflate(R.layout.pop_layout, null);
        CustomPopWindow popWindow = new CustomPopWindow.PopupWindowBuilder(this)
                .setView(contentView)//显示的布局，还可以通过设置一个View
                .setFocusable(true)//是否获取焦点，默认为ture
                .setOutsideTouchable(true)//是否PopupWindow 以外触摸dissmiss
                .size(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT)//显示大小
                .setOnDissmissListener(mDismissListener)
                .setAnimationStyle(R.style.pop_anim_style)
                .create()//创建PopupWindow
                .showAtLocation(activity_main, Gravity.BOTTOM | Gravity.CENTER_HORIZONTAL, 0, 0);
                
        changeBgColor(DARK_ALPHA);//背景半透明
                
        Button btn_one = (Button) contentView.findViewById(R.id.btn_one);
        Button btn_two = (Button) contentView.findViewById(R.id.btn_two);
        Button btn_three = (Button) contentView.findViewById(R.id.btn_three);
        btn_one.setOnClickListener(mListener);
        btn_two.setOnClickListener(mListener);
        btn_three.setOnClickListener(mListener);
        
        PopupWindow.OnDismissListener mDismissListener = new PopupWindow.OnDismissListener() {
           @Override
           public void onDismiss() {
            changeBgColor(BRIGHT_ALPHA);//背景全透明
           }
        };
        
        View.OnClickListener mListener = new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                switch (view.getId()) {
                    case R.id.btn_one:
                        popWindow.dissmiss();
                        changeBgColor(BRIGHT_ALPHA);
                        break;
                    case R.id.btn_two:
                        popWindow.dissmiss();
                        changeBgColor(BRIGHT_ALPHA);
                        break;
                    case R.id.btn_three:
                        popWindow.dissmiss();
                        changeBgColor(BRIGHT_ALPHA);
                        break;
                }
            }
        };
        
    /**
     * 设置弹出消失时的背景
     *
     * @param brightAlpha
     */
    private void changeBgColor(float brightAlpha) {
        this.getWindow().addFlags(WindowManager.LayoutParams.FLAG_DIM_BEHIND);
        changeAlpha(brightAlpha, true);
    }

    private void changeAlpha(float alpha, boolean anim) {
        final WindowManager.LayoutParams layoutParams = getWindow().getAttributes();
        if (anim) {
            float startAlpha = layoutParams.alpha;
            final ValueAnimator animation = ValueAnimator.ofFloat(startAlpha, alpha);
            animation.setDuration(300);
            animation.start();
            animation.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
                @Override
                public void onAnimationUpdate(ValueAnimator valueAnimator) {
                    layoutParams.alpha = (Float) valueAnimator.getAnimatedValue();
                    getWindow().setAttributes(layoutParams);
                }
            });
            return;
        }
        layoutParams.alpha = alpha;
        getWindow().setAttributes(layoutParams);
    }
