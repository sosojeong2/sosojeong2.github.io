---
layout: post
title: "[spring]예외처리"
date: 2020-12-01 14:27:23 +0900
category: springboot
---

# 예외처리

## Spring에서 Exception처리하는데 3가지 방법이 있다.
* 전역처리 Global Level using - @ControllerAdvice
* 컨트롤러에서 처리 Controller Level using - @ExceptionHandler
* 메소드단위 처리 Method Level using - try/catch     

   
* * *      
@ControllerAdvice는 AOP를 이용하여 공통적인 예외상황에 대해서 별도로 분리하여 처리하는 방법이다.

``` java

@RestControllerAdvice
public class ExceptionAdvice {

  private static final Logger logger = LoggerFactory.getLogger(ExceptionAdvice.class);
  private final ResponseService responseService;
  private final MessageSource messageSource; 

  //아래 예외를 제외한 모든 에러 
  @ExceptionHandler(Exception.class) 
  protected ResponseResult handleException(Exception e) {
    logger.error("handleException", e);
    CommonResult failResult = responseService.getFailResult(Integer.valueOf(getMessage("unKnown.code")), getMessage("unKnown.msg"));
    return responseService.setResult(failResult);

  }

  //필수파라미터 에러 
  @ExceptionHandler({RequiredParamException.class}) 
  protected ResponseResult handleRequiredParamException(RequiredParamException e) {
    logger.error("handleRequiredParamException", e);
    CommonResult failResult = responseService.getFailResult(Integer.valueOf(getMessage("requiredParam.code")), getMessage("requiredParam.msg"));
    return responseService.setResult(failResult);
  }

   //code정보, 추가 argument로 현재 locale에 맞는 메시지를 조회.
   private String getMessage(String code, Object[] args) {
       return messageSource.getMessage(code, args, LocaleContextHolder.getLocale());
   }
}

```

클래스 선언 시 @RestControllerAdvice 을 붙여주면 controller에서 발생한 예외를 처리하는 클래스가 된다.
내부에 선언된 메서드에 @@ExceptionHandler 가 있는데 속성값으로 예외타입을 지정하여 해당 예외가 발생했을 때 수행할 메서드가 된다.

try/ catch 사용법 예시 
``` java

  try {
      service.saveMember(member);
    } catch (RequiredParamException re) {
        CommonResult failResult = responseService.getFailResult(Integer.valueOf(getMessage("requiredParam.code")), getMessage("requiredParam.msg"));
        return responseService.setResult(failResult);
    } catch (Exception e) {
        CommonResult failResult = responseService.getFailResult(Integer.valueOf(getMessage("unKnown.code")), getMessage("unKnown.msg"));
        return responseService.setResult(failResult);
    }

```

try/catch를 통해 예외를 처리하면 직접 처리한 것이 우선순위로 작동하게 된다.
그러나 try/catch를 반복적으로 사용하면 가독성이 떨어진다.



