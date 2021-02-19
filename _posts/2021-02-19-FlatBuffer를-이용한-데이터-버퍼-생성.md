---
layout: post
title : "FlatBuffer를 이용한 데이터 버퍼 생성"
date:   2021-02-19
excerpt: "FlatBuffer 개념과 실제 사용법 정리"
tags: [flatbuffer]
comments: true
---

# FlatBuffer를 이용한 데이터 버퍼 생성

## 개념
- 테이블 : 데이터 버퍼로 생성할 데이터 정의
- 구조체 : 테이블에 포함되는 사용자 정의 자료형(ex : Vector)

## 사용법
- FlatBufferBuilder를 이용해 버퍼 생성
- 각 테이블 클래스를 이용해 FlatBufferBuilder에 테이블 데이터 입력

## FlatBufferBuilder
- 버퍼 생성 및 버퍼 데이터 값 입력 제공
- 각 테이블 클래스에 대한 Start{테이블명} ~ End{테이블명} or Create{테이블명} 함수를 사용하여 버퍼에 데이터값 입력
- 버퍼에 모든 데이터 입력 완료 후 Finish 함수를 호출하여, 버퍼 데이터 메타정보 저장 (버퍼 오프셋, 버퍼 길이(기본 false))
- SizedByteArray 함수를 호출해 서버에 전송할 최종 byte Array 생성

## FlatBufferBuilder 내부 처리
- Start{테이블명} ~ End{테이블명} or Create{테이블명} 함수 사용 시 필드에 대한 메타정보도 버퍼에 입력됨
- 버퍼에 테이블 필드 값 저장 시 버퍼 끝부터 역순으로 저장됨
- 버퍼 사이즈가 부족한 경우 버퍼 크기 현재 크기에서 2배 증가 (배열 생성 및 복사가 일어나, 초기 생성 시 알맞은 사이즈 값 조정 필요)

## 서버 판별용 데이터 저장
- 테이블 타입 및 파싱할 버퍼 사이즈를 판별하기 위해 버퍼에 해당 정보 추가 필요
- 버퍼 길이 추가 :  FlatBufferBuilder Finish함수 대신 FinishSizePrefixed함수를 사용하면 버퍼 최상위에 길이 정보(int)가 추가됨, int 외에 타입은 직접 데이터 삽입 필요
- 테이블 타입 : FlatBufferBuilder를 사용해 직접 데이터 삽입 필요(ex : PutShort, PutInt)
